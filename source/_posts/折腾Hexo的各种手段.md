---
title: 折腾Hexo的各种手段
date: 2019-01-11 14:36:55
categories:
- Hexo
tags:
- Hexo
- WEB前端
---

折腾Hexo的各种手段
<!-- more --> 




# 一、这是一篇测试文章

```bash
hexo new "Hexo之开始折腾"
```

# 二、发布到服务器和Github

```bash
hexo clean
hexo deploy
```

# 三、插入网易云音乐
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=571779101&auto=1&height=66"></iframe>

# 四、静态资源压缩

安装gulp插件：

```bash
cnpm install gulp -g
cnpm install gulp-minify-css gulp-uglify gulp-htmlmin gulp-htmlclean gulp-imagemin -S
```

新建 `gulpfile.js` 文件，文件内容如下（以下代码适用于 gulp@3 版本）：

```javascript
var gulp = require('gulp');
var minifycss = require('gulp-minify-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
var imagemin = require('gulp-imagemin');

// 压缩css文件
gulp.task('minify-css', function () {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss())
        .pipe(gulp.dest('./public'));
});

// 压缩html文件
gulp.task('minify-html', function () {
    return gulp.src('./public/**/*.html')
        .pipe(htmlclean())
        .pipe(htmlmin({
            removeComments: true,
            minifyJS: true,
            minifyCSS: true,
            minifyURLs: true,
        }))
        .pipe(gulp.dest('./public'))
});

// 压缩js文件
gulp.task('minify-js', function () {
    return gulp.src(['./public/**/.js', '!./public/js/**/*min.js'])
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});

// 压缩 public/demo 目录内图片
gulp.task('minify-images', function () {
    gulp.src('./public/demo/**/*.*')
        .pipe(imagemin({
            optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
            progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
            interlaced: false, //类型：Boolean 默认：false 隔行扫描gif进行渲染
            multipass: false, //类型：Boolean 默认：false 多次优化svg直到完全优化
        }))
        .pipe(gulp.dest('./public/uploads'));
});

// 默认任务
gulp.task('default', [
    'minify-html', 'minify-css', 'minify-js', 'minify-images'
]);
```

只需要每次在执行 `generate` 命令后执行 `gulp` 就可以实现对静态资源的压缩，压缩完成后执行 `deploy` 命令同步到服务器：

```bash
hexo clean
hexo generate
gulp
hexo deploy
```

**压缩了，好用，加载速度杠杠**
修改 `package.json` ，再偷懒一下~
```json
"scripts": {
    "ok": "hexo clean & hexo generate & gulp & hexo deploy"
}
```
```bash
npm run ok
```
# 五、主页文章预览之加载更多

使用如下即可
```html
<!-- more --> 
```