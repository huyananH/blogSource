---
title: 我自己的第一个node.js项目
date: 2017-12-04 08:54:02
tags:
  - Node.js
  - Express
  - Scss
---

<img src="/assets/postImg/nodefirstprojectLogo.jpeg" width="350px" height="350px">

### 一、 前言

今天，看到了一个笑话，是这样写的
> 给每个员工发一只猫，离职的话要退还猫给公司，这样可以启动稳定团队的效果

想想，很有道理，哈哈哈......

<!-- more -->

### 二、 生成应用骨架
* 通过应用生成器工具 express 可以快速创建一个应用的骨架。
通过如下命令安装：
```
npm install express-generator -g
```

* -h 选项可以列出所有可用的命令行选项：
```
express -h
```
结果：
```
Usage: express [options] [dir]

Options:

  -h, --help          output usage information
  -V, --version       output the version number
  -e, --ejs           add ejs engine support (defaults to jade)
      --hbs           add handlebars engine support
  -H, --hogan         add hogan.js engine support
  -c, --css <engine>  add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain css)
      --git           add .gitignore
  -f, --force         force on non-empty directory
```

* 例如，下面的示例就是在当前工作目录下创建一个命名为 firstnode 的应用。
```
express firstnode
```

* 安装所有依赖包
```
cd firstnode
npm install
```

* 启动这个应用（MACOS或Linux）
```
DEBUG=firstnode npm start
```

* 启动这个应用（Windows）
```
set DEBUG=myapp & npm start
```

* 然后在浏览器中打开 http://localhost:3000/ 网址就可以看到这个应用了。i

通过 Express 应用生成器创建的应用一般都有如下目录结构：
```
.
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.jade
    ├── index.jade
    └── layout.jade

7 directories, 9 files
```

* 添加ej、css依赖
```
express -e
express -c
```

* 安装grunt
```
npm install grunt
npm install grunt-cli
```

* 安装grunt依赖
```
npm install grunt-contrib-watch
npm install grunt-contrib-concat
npm install grunt-contrib-sass
npm install grunt-contrib-cssmin
```

### 三、 编写Gruntfile文件

例子：
```
module.exports = function(grunt) {
    // 使用严格模式
    'use strict';

    // 这里定义我们需要的任务
    grunt.initConfig({
    //读取json文件
    pkg:grunt.file.readJSON('package.json'),

    // Metadata.
    meta: {
        basePath: '.',
        srcPath: 'public/stylesheets/scss/',
        deployPath: 'public/stylesheets/css/'
    },

    banner: '/*! <%= pkg.name %> - v<%= pkg.version %> - ' +
    '<%= grunt.template.today("yyyy-mm-dd") %>\n' +
    '* Copyright (c) <%= grunt.template.today("yyyy") %> ',

    // Task configuration.
    sass: {
        dist: {
            files: {
                '<%= meta.deployPath %>index.css': '<%= meta.srcPath %>index.scss'
            }
        }
    },
    //合并文件
    concat: {
        css: {
          src: ['public/stylesheets/css/*.css'],
          dest: 'public/dist/css/style.css'
        }
    },
    //压缩文件 css
    cssmin: {
        css: {
          src: 'public/dist/css/style.css',
          dest: 'public/dist/css/style.min.css'
        }
    },

    watch: {
        scripts: {
            files: [
                '<%= meta.srcPath %>/**/*.scss'
            ],
            tasks: ['sass','concat:css','cssmin:css']
        }

    }


  });

    // These plugins provide necessary tasks.
    grunt.loadNpmTasks('grunt-contrib-sass');
    grunt.loadNpmTasks('grunt-contrib-watch');
    grunt.loadNpmTasks('grunt-contrib-concat');
    grunt.loadNpmTasks('grunt-contrib-cssmin');

    // Default task.
    //eg: grunt sass
    //eg: grunt watch
    grunt.registerTask('css',['sass','concat:css','cssmin:css']);
    grunt.registerTask('default', ['css', 'watch']);
};
```

* 执行
```
//默认配置
grunt
```
