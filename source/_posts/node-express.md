---
title: 一个菜鸟对Grunt的告白
date: 2017-11-30 10:37:55
tags:
  - Node.js
  - 框架
---

### 前言

以前，从没接触过动态语言，所以于我而言，Node.js学起来，还是没有那么简单的。
我怀揣着一颗好奇的心，开始学习Node.js。

<!-- more -->

### 1. npm

在写node.js，见到了npm......

npm是node的包管理器，我觉得它就类似于Maven。

npm init 自动生成package.json

### 2. bower

bower前台包管理器

bower init 自动生成bower.json

### 3. grunt 中Gruntfile.js详解

这个Gruntfile.js用到了5个插件：
* grunt-contrib-uglify  --用来压缩文件
* grunt-contrib-qunit   
* grunt-contrib-concat  --用来合并文件
* grunt-contrib-jshint  
* grunt-contrib-watch   --用来监听文件，如果有文件改变，就会触发回调方法进行相应的处理

1. “wapper”函数，它包含整个Grunt配置信息。
module.exports = function(grunt) {}

2. 初始化configuration，并读入package.json
在这个函数中，我们可以初始化configuration对象：
  grunt.initConfig({});

接下来，从package.json文件读入项目配置信息，并存入pkg属性内。这样，我们就可以访问到package.json文件中列出的属性，如下：
pkg:grunt.file.readJSON('package.json');

<hr>

到目前为止，我们看到的配置文件是：
```
module.exports = function(grunt) {
  grunt.initConfig({
    pkg:grunt.file.readJSON('package.json');
    });
};
```

3. 为任务定义相应的配置

```
contact: {
  options: {
    //定义用于插入合并输出文件之间的字符
    separator: ';'
  }
  dist: {
    //将要合并的文件
    src: ['src/**/*.js'],
    //合并后的Js文件的存放为止
    dist: 'dist/<%= pkg.name %>.js'
  }
}
```
这里使用了pkg.name来访问我们刚才引入并存储在pkg属性中的package.json文件信息，它会被解析为一个JavaScript对象。Grunt自带的有一个简单的模板引擎用于输出配置对象(这里是指package.json中的配置对象)，这里我让concat任务将所有存在于src/目录下以.js结尾的文件合并起来，然后存储在dist目录中，并以项目名来命名。

简单写
```
concat: {
            js: {
                src: ['src/js/*.js'],
                dest: 'build/js/global.js'
            },
            css: {
                src: ['src/css/*.css'],
                dest: 'build/css/style.css'
            }
        },
```

4. 配置uglify插件，它的作用是压缩文件：
```
uglify: {
　　options: {
　　　　// 此处定义的banner注释将插入到输出文件的顶部
　　　　banner: '/*! <%= pkg.name %> <%= grunt.template.today("dd-mm-yyyy") %> */\n'
　　},
　　dist: {
　　　　files: {
　　　　　　'dist/<%= pkg.name %>.min.js': ['<%= concat.dist.dest %>']
　　　　}
　　}
}
```

这里我们让uglify在dist/目录中创建了一个包含压缩结果的JavaScript文件。注意这里我使用了<%= concat.dist.dest>，因此uglify会自动压缩concat任务中生成的文件。

简单写
```
uglify: {
            js: {
                src: 'build/js/global.js',
                dest: 'build/js/global.min.js'
            }
        },
```



QUnit插件的设置非常简单，你只需要给它提供用于测试运行的文件的位置，注意这里的QUnit是运行在HTML文件上的。
```
qunit: {
　　files: ['test/**/*.html']
},
```

5. JSHint JavaScript语法、风格检查工具

JSHint是一个javaScript语法和风格的检查工具，但检查不出逻辑问题。
```
jshint: {
  // define the files to lint
  files: ['gruntfile.js', 'src/**/*.js', 'test/**/*.js'],
  // configure JSHint (documented at http://www.jshint.com/docs/)
  options: {
      // more options here if you want to override JSHint defaults
    globals: {
      jQuery: true,
      console: true,
      module: true
    }
  }
}
```
JSHint只需要一个文件数组(也就是你需要检测的文件数组)， 然后是一个options对象(这个对象用于重写JSHint提供的默认检测规则)。你可以到JSHint官方文档站点中查看完整的文档。如果你乐于使用JSHint提供的默认配置，那么在Gruntfile中就不需要重新定义它们了。

简单写
```
jshint: {
            all: ['./build/js/global.js']
        },
```


6. watch 监听

```
watch: {
  files: ['<%= jshint.files %>'],
  tasks: ['jshint', 'qunit']
}
```

你可以在命令行使用grunt watch来运行这个任务。当它检测到任何你所指定的文件(在这里我使用了JSHint任务中需要检测的文件)发生变化时，它就会按照你所指定的顺序执行指定的任务(在这里我指定了jshint和qunit任务)。

接下来, 我们还要加载所需要的Grunt插件。 它们应该已经全部通过npm安装好了。

例子：
```
watch: {
            scripts: {
                files: ['src/js/*.js', 'src/css/*.css'],
                tasks: ['concat', 'jshint', 'cssmin', 'uglify:js']
            },
            livereload: {
                options: {
                    livereload: '<%= connect.options.livereload %>'
                },
                files: [
                    'index.html',
                    'build/css/style.min.css',
                    'build/js/global.min.js'
                ]
            }
        },
```

7. 加载任务插件

//加载任务插件
```
grunt.loadNpmTasks('grunt-contrib-concat');
grunt.loadNpmTasks('grunt-contrib-jshint');
grunt.loadNpmTasks('grunt-contrib-uglify');
grunt.loadNpmTasks('grunt-contrib-watch');
grunt.loadNpmTasks('grunt-contrib-connect');
grunt.loadNpmTasks('grunt-css');
```

8. 注册任务

```
    //注册任务，执行grunt js 会concat  jshint uglify
    grunt.registerTask('js', ['concat:js', 'jshint', 'uglify:js']);
    //注册任务，执行grunt css 会concat cssmin
    grunt.registerTask('css', ['concat:css', 'cssmin:css']);
    //注册任务，执行grunt  会 css js connect watch
    grunt.registerTask('default', ['css', 'js', 'connect', 'watch']);
```

### 4. 例子

*-- [这里是grunt的例子](https://github.com/huyananH/grunt_demo)*
