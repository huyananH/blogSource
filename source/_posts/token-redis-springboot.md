---
title: 基于Springboot及Redis的Token令牌
date: 2017-10-23 09:28:21
comments: true
reward: true
tags:
  -SpringBoot
  -Token
  -Redis
---

### 一、前言

在之前，已经介绍了Redis的安装，接下来，介绍一下基于SpringBoot和Redis的Token认证。

<!-- more -->

交互流程：
* 客户端通过登录请求提交用户名和密码，服务端验证成功之后，生成一个Token并和该用户建立连接，传给客户端。
* 客户端在接下来的请求中，都会携带该Token，服务端通过解析Token，验证用户的登录状态。
* 当用户退出登录、被顶号，或者长时间未进行操作，用户就需要重新登录。

### 二、SpringBoot整合Redis
  1. 在pom.xml文件中添加依赖
  ```
  <!-- redis依赖 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-redis</artifactId>
			<version>1.4.1.RELEASE</version>
		</dependency>
  ```

  2. 配置Redis，在application.properties文件中

  ```
  # redis 数据库索引，默认为0
  spring.redis.database=0
  # redis 服务器地址
  spring.redis.hsot=192.168.1.117
  # Redis 服务器端连接端口
  spring.redis.port=6379
  # redis 服务器连接密码
  spring.redis.password=
  # 连接池最大连接数
  spring.redis.pool.max-active=8
  # 连接池最大阻塞等待时间（使用负值表示没有）
  spring.redis.pool.max-wait=-1
  # 连接池中的最大空闲连接
  spring.redis.pool.max-idle=8
  # 连接池中的最小空闲连接
  spring.redis.pool..min-idle=0
  # 连接超时时间（毫秒）
  spring.redis.timeout=0
  ```

  3. Redis的配置文件
  ```
  package cn.dfusion.config;
  import java.lang.reflect.Method;
  import java.util.HashMap;
  import java.util.Map;

  import org.springframework.cache.CacheManager;
  import org.springframework.cache.annotation.CachingConfigurerSupport;
  import org.springframework.cache.annotation.EnableCaching;
  import org.springframework.cache.interceptor.KeyGenerator;
  import org.springframework.context.annotation.Bean;
  import org.springframework.context.annotation.Configuration;
  import org.springframework.data.redis.cache.RedisCacheManager;
  import org.springframework.data.redis.connection.RedisConnectionFactory;
  import org.springframework.data.redis.core.RedisTemplate;
  import org.springframework.data.redis.core.StringRedisTemplate;
  import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;

  import com.fasterxml.jackson.annotation.JsonAutoDetect;
  import com.fasterxml.jackson.annotation.PropertyAccessor;
  import com.fasterxml.jackson.databind.ObjectMapper;

  @Configuration
  @EnableCaching // 启用缓存，这个注解很重要；
  public class RedisConfig extends CachingConfigurerSupport {

      /**
       * 生成key的策略
       *
       * @return
       */
      @Bean
      public KeyGenerator keyGenerator() {
          return new KeyGenerator() {
              @Override
              public Object generate(Object target, Method method, Object... params) {
                  StringBuilder sb = new StringBuilder();
                  sb.append(target.getClass().getName());
                  sb.append(method.getName());
                  for (Object obj : params) {
                      sb.append(obj.toString());
                  }
                  return sb.toString();
              }
          };
      }

      /**
       * 管理缓存
       *
       * @param redisTemplate
       * @return
       */
      @SuppressWarnings("rawtypes")
      @Bean
      public CacheManager cacheManager(RedisTemplate redisTemplate) {
          RedisCacheManager rcm = new RedisCacheManager(redisTemplate);
          //设置缓存过期时间
          // rcm.setDefaultExpiration(60);//秒
          //设置value的过期时间
          Map<String,Long> map=new HashMap<String, Long>();
          map.put("test",60L);
          rcm.setExpires(map);
          return rcm;
      }

      /**
       * RedisTemplate配置
       * @param factory
       * @return
       */
      @Bean
      public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory factory) {
          StringRedisTemplate template = new StringRedisTemplate(factory);
          Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
          ObjectMapper om = new ObjectMapper();
          om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
          om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
          jackson2JsonRedisSerializer.setObjectMapper(om);
          template.setValueSerializer(jackson2JsonRedisSerializer);
          //如果key是String 需要配置一下StringSerializer,不然key会乱码 /XX/XX
          template.afterPropertiesSet();
          return template;
      }
  }
  ```

  4. 测试springBoot整合Redis是否成功
  ```
  package cn.dfusion.utils;

  import org.junit.Test;
  import org.junit.runner.RunWith;
  import org.springframework.beans.factory.annotation.Autowired;
  import org.springframework.boot.autoconfigure.SpringBootApplication;
  import org.springframework.data.redis.core.RedisTemplate;
  import org.springframework.data.redis.core.StringRedisTemplate;
  import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

  @RunWith(SpringJUnit4ClassRunner.class)
  @SpringBootApplication
  public class RedisUtilsTest {

      @Autowired
     private StringRedisTemplate stringRedisTemplate;

      @Autowired
      private RedisTemplate<String, String> template;

      @Test
      public void testObj() throws Exception {
          stringRedisTemplate.opsForValue().set("name","huyanan");
          System.out.println("添加成功");
      }

      @Test
      public void delete() {
          template.delete("name");
      }

  }
  ```
  * 执行完testObj()后，打开Redis客户端
    ```
    # 进入redis文件夹
    cd /usr/local/redis-4.0.2
    # 打开客户端
    ./bin/redis-cli
    ```

  * 执行命令，查看是否有添加的这条数据
  ```
  exists name
  ```
  ![查看这条数据是否存在](/assets/postImg/testObj.jpg)

  * 删除数据时,执行delete()方法
  报错内容：
  解决Redis之MISCONF Redis is configured to save RDB snapshots, but is currently not able to persist on disk. Commands that may modify the data set are disabled. Please check Redis logs for details about the error.

  解决方案在博客里也有写到

### 三、RESTful登录设计代码
  1. 服务端生成的Token一般是随机非重复字符串，根据应用不同的安全性，可以添加时间戳（或者其他信息），该例子是以userId,和Token以‘-’连接
  TokenModel.java
  ```
  package cn.dfusion.entity;

  public class TokenModel {

      private long userId;

      private String token;

      public TokenModel() {
      }

      public TokenModel(long userId, String token) {
          this.userId = userId;
          this.token = token;
      }

      public long getUserId() {
          return userId;
      }

      public void setUserId(long userId) {
          this.userId = userId;
      }

      public String getToken() {
          return token;
      }

      public void setToken(String token) {
          this.token = token;
      }
  }
  ```

  2. redis是key-value非关系型数据库，这里使用spring-data-redis封装TokenManager对Token的操作。
  TokenManager.java
  ```
  package cn.dfusion.utils;

  import cn.dfusion.entity.TokenModel;

  public interface TokenManager {

      public static  final String SERVER_NAME = "cn.dfusion.utils.impl.TokenManagerImpl";

      /**
       * 创建一个Token，关联指定用户
       * @param userId 用户ID
       * @return token 生成的Token
       */
      TokenModel createToke(long userId);

      /**
       * 检测Token是否有效
       * @param model Token
       * @return 是否有效
       */
      boolean checkToken(TokenModel model);

      /**
       * 在字符串中解析Token
       * @param authentication 加密后的Token字符串
       * @return TokenModel
       */
      TokenModel getToken(String authentication);

      /**
       * 清除指定Token
       * @param userId 用户ID
       */
      void deleteToke(long userId);
  }

  ```
