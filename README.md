#一、gulp安装构建
***
###1.安装node.js
   [node.js下载官网](https://nodejs.org/en/)
###2.使用命令行
   2.1 可以在windows系统下的命令提示符cmd（window+R，回车）中操作，也可在安装好的git的git bash中操作。
   2.2 常用命令：
        `node -v`查看安装的node.js版本，如能显示版本号，说明安装成功；
        `npm -v`查看npm版本号，npm在安装node.js时一同安装的包管理器；
        `cd`定位到目录，用法:cd+路径；
        `dir`列出文件列表；
        `cls`清空命令提示符窗口内容。
###3.npm介绍
   3.1、说明：npm（node package manager）nodejs的包管理器，用于node插件管理（包括安装、卸载、管理依赖等）；
   3.2、输入命令行`npm init`，就可以在项目根目录中自动生成配置文件 `package.json`。
          <span style='color:red;'> PS：</span> 为什么要保存至package.json？因为node插件包相对来说非常庞大，所以不加入版本管理，将配置信息写入package.json并将其加入版本管理，其他开发者对应下载即可（命令提示符执行`npm install`，则会根据package.json下载所有需要的包，`npm install --production`只下载dependencies节点的包）。
 
<span style='color:red;'> 注意：</span>如若再npm下载插件过程中下载缓慢或出现异常，可选装国内淘宝团队的cnpm来代替npm（http://npm.taobao.org），安装命令为`npm install cnpm -g --registry=https://registry.npm.taobao.org`。
###4.安装gulp
  4.1 全局安装：`npm install -g gulp`。可用`gulp -v`查看版本号确认是否安装成功。
  4.2 本地安装：`npm install --save-dev gulp`
 <span style='color:red;'>  PS：</span>我们全局安装了gulp，项目也安装了gulp，全局安装gulp是为了执行gulp任务，本地安装gulp则是为了调用gulp插件的功能。

###5.安装依赖（插件）
            sass的编译                                        （gulp-ruby-sass）
            自动添加css前缀                                   （gulp-autoprefixer）
            压缩css                                              (gulp-minify-css)
            压缩html                                              (gulp-minify-html)
            js代码校验                                           （gulp-jshint）
            合并js文件                                           （gulp-concat）
            压缩js代码                                           （gulp-uglify）
            压缩图片                                              （gulp-imagemin）
            自动刷新页面                                       （gulp-livereload）
            图片缓存，只有图片替换了才压缩                       (gulp-cache)
            更改提醒                                              （gulp-notify）
            清除文件                                               （del）

 插件安装：`npm install gulp-jshint gulp-sass gulp-concat gulp-uglify gulp-rename --save-dev`
 插件卸载：`npm uninstall <name> [-g] [--save-dev]`     PS：不要直接删除本地插件包
卸载全部插件：` npm uninstall gulp-less gulp-uglify gulp-concat`     ……???太麻烦
 使用npm更新插件：`npm update <name> [-g] [--save-dev]`
更新全部插件：`npm update [--save-dev]`
 当前目录已安装插件：`npm list`

 <span style='color:red;'>  PS：</span>
 * 安装js代码校验插件jshint时，安装命令并不是`npm install --save-dev gulp-jshint`，而是`npm install --save-dev jshint gulp-jshint`。
 * 当你为你的模块安装一个依赖模块(插件)时，正常情况下你得先安装他们（在模块根目录下 `npm install module-name`），然后连同版本号手动将他们添加到模块配置文件package.json中的依赖里（`dependencies`）。
 &nbsp;
   `-save`和`save-dev` 可以省掉你手动修改package.json文件的步骤。
   `npm install module-name -save`  自动把模块和版本号添加到dependencies部分
   `npm install module-name -save-dve`  自动把模块和版本号添加到devdependencies部分

###6.构建项目
在项目根目录下新建一个`gulpfile.js`文件，黏贴以下代码
```
// gulp依赖引用
var gulp = require('gulp');
var rename = require('gulp-rename');
var uglify = require('gulp-uglify');
var concat = require('gulp-concat');
var minifycss = require('gulp-minify-css');
var minifyhtml = require('gulp-minify-html');
var imagemin = require('gulp-imagemin');

//压缩js文件
gulp.task('compress',function(){                 //创建一个任务，compress为任务的名字，名字可以任取
    return gulp.src(['./src/js/index.js','./src/js/login.js'])               //导入.js处理文件
    .pipe(uglify())           //执行js的压缩
    .pipe(concat('main.js'))      //将两文件合并成一个文件，并命名为main.js
    //重命名
    .pipe(rename({
    	suffix:'-min'          //在原来文件名后面加上-min字符串 
    }))
    .pipe(gulp.dest('dist/js'));            //把压缩的文件导出到dist/js文件夹里
});

gulp.watch('./src/js/*.js',['compress']);   //当文件改变时，自动执行compress任务

//压缩css文件
gulp.task('minifycss',function(){
	gulp.src('./src/css/index.css')
	.pipe(minifycss())
	.pipe(gulp.dest('dist/css'));
});
//压缩html文件
gulp.task('minifyhtml',function(){
	gulp.src('./src/html/login.html')
	.pipe(minifyhtml())
	.pipe(gulp.dest('dist/html'));
});

//压缩图片
gulp.task('imagemin',function(){
	gulp.src('./src/img/*.+(jpg|png)')
	.pipe(imagemin({

	}))
	.pipe(gulp.dest('dist/img'));
});
//默认的一个任务
gulp.task('default',['compress','minifycss','minifyhtml','imagemin']);
```
###7.执行
 敲入命令`gulp`，此时压缩后的文件就存在于dist文件夹里；`gulp 任务名`可以单独的执行某个任务。
