Gulp学习
gulp是基于nodejs的项目自动化构建工具，开发者可以使用它在项目开发过程中自动执行常见任务。

如何利用gulp构建项目

安装nodejs
安装地址： https://nodejs.org/en/
检测是否安装成功,打开cmd命令提示符窗口
$ node -v
能够正常返回版本号则安装成功

安装gulp
npm的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已，一般是全局安装。
$ npm install gulp -g  （这是第一次安装gulp时才需要全局安装的）

作为项目的开发依赖（devDependencies）安装：
全局安装成功之后，我们还需要本地安装gulp作为项目依赖模块使用。这里首先创建一个简单的测试项目gulpStydy。

cmd命令行进入项目目录，执行$ npm init，项目初始化，生成package.json文件
本地安装：$ npm install gulp --save-dev，成功之后该模块包（node_modules文件夹）将在当前目录下被安装，并且作为项目的依赖模块使用。同时，项目根目录中将自动生成一个package.json文件，这个文件将记录该项目的重要信息，尤其是项目的依赖模块。

配置gulp文件
在项目根目录下创建一个名为 gulpfile.js 的文件：
var gulp = require('gulp');
gulp.task('default', function() {
  // 将你的默认的任务代码放在这
});

运行gulp
命令行执行$ gulp
默认的名为 default 的任务（task）将会被运行，在这里，这个任务并未做任何事情。想要单独执行特定的任务（task），输入 $ gulp jsmin

API gulp的API
参考：http://www.gulpjs.com.cn/docs/api/

gulp插件
实际上，完整的构建一个项目依靠的往往不是gulp本身，而是依靠全球范围内的gulp插件开发者上传的众多优秀模块，而gulp更像一个平台。想了解最新的gulp插件可以参照： http://gulpjs.com/plugins/ 这里，将为大家介绍一些常见的插件及其使用方法： #### 安装插件 #### 简单安装一个插件，比如：
$ npm install gulp-uglify --save-dev  （一个js压缩插件）
这里推荐将--save-dev加上，它将自动将依赖添加到项目的package.json文件中。 你也可以一次性安装多个插件，比如：
$ npm install gulp-uglify gulp-minify-css gulp-imagemin --save-dev

#### 使用插件 #### 安装完插件之后，就可以在配置文件中使用插件，比如使用js压缩插件：
```
// 安装重命名插件：
$ npm install --save-dev gulp-rename   //官网上安装插件是这样写的

// 需要用到哪个插件，则需require哪个插件：
var gulp = require('gulp');
var uglify = require('gulp-uglify');  //js压缩
var htmlmin = require('gulp-htmlmin'); //html压缩
var minifycss = require('gulp-minify-css');//css压缩
var autoprefixer = require('gulp-autoprefixer');//css自动加前缀
var rename = require("gulp-rename");//重命名
var watch = require('gulp-watch');//监听
var swig = require('gulp-swig');//模板引擎

// 定义jsmin任务(task):
gulp.task('jsmin', function() {
    //将你的默认的任务代码放在这里,命令行gulp执行的aaa任务
    return gulp.src('app/js/*.js')
        .pipe(rename(function (path) {
            path.extname = ".min.js";
        }))
        .pipe(uglify())
        .pipe(gulp.dest('output/js/'))
})

// 定义cssmin任务：
gulp.task('cssmin', function() {
    return gulp.src('app/css/*.css')
        .pipe(autoprefixer({
            browsers: ['Android >= 4.0','>5%'],//主流浏览器的两个版本
            cascade: false,//
            remove: true//清除过时的前缀，默认为true
        }))
        .pipe(rename(function (path) {
            path.extname = ".min.css";
        }))
        .pipe(minifycss({keepBreaks:true}))
        //.pipe(minifycss())
        .pipe(gulp.dest('output/css/'))
})

// 使用swig模板：
gulp.task('swig', function() {
    return gulp.src('app/page/*.html')
        .pipe(swig({defaults:{cache:false}}))//Avoid caching when watching/compiling html templates with BrowserSync, etc.
        .pipe(gulp.dest('app/views'));
})
// 这个模板不太懂使用！！



// 安装监听插件：
$ npm install gulp-watch --save-dev

// 定义watch任务(task):
gulp.task('watch', function() {
    gulp.watch('app/js/*.js',['jsmin']);
})
```
