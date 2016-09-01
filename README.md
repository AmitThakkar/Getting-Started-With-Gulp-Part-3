#Getting Started with GulpJS Part-3

####This repository contains getting started guild with GulpJS Part 3.

In my previous [blog]() we write **Gulp** task to minify all the ```.js``` files then concat them into one ```all.js``` file, and watch all the ```.js``` files, if any change happen to any ```.js``` file, we were rerunning the minify and concat task and we were doing live reloading the **HTML** on chrome browser.

Here is code of previous **gulpfile.js**:
```JavaScript
var gulp = require('gulp'),
    uglify = require('gulp-uglify'),
    concat = require('gulp-concat'), // Requiring gulp-concat task.
    livereload = require('gulp-livereload'); // Requiring gulp-livereload task.
function errorLog(error) {
    console.error(error);
    this.emit('end');
}
gulp.task('scripts', function () {
    gulp.src('js/*.js')
        .pipe(uglify())
        .on('error', errorLog)
        .pipe(concat('all.js')) // Adding concat task here
        .pipe(gulp.dest('build/js'))
        .pipe(livereload()); // Adding livereload task here. Which creates a livereload server.
});
gulp.task('watch', function () {
    livereload.listen(); // Calling lister on livereload task, which will start listening for livereload client.
    gulp.watch('js/*.js', ['scripts']);
});
gulp.task('default', ['scripts', 'watch']);
```

Now in this blog, we will run a **NodeJS** application with **GulpJS**. And on successful start, we open application URL on google chrome browser by **Gulp** task so developer should not require to open that URL manually. And This time we will add **JSHint**, which will tell us compile time errors before running the application.

Lets install **gulp.nodemon**, **gulp-jshint** and **gulp-open** modules and add then require them.

```JavaScript
var gulp = require('gulp'),
    uglify = require('gulp-uglify'),
    concat = require('gulp-concat'), // Requiring gulp-concat task.
    livereload = require('gulp-livereload'), // Requiring gulp-livereload task.
    nodemon = require('gulp-nodemon'),
    jshint = require('gulp-jshint'),
    open = require("gulp-open");
function errorLog(error) {
    console.error(error);
    this.emit('end');
}
gulp.task('scripts', function () {
    gulp.src('public/scripts/*.js')
        .pipe(uglify())
        .on('error', errorLog)
        .pipe(concat('all.js')) // Adding concat task here
        .pipe(gulp.dest('build/scripts'))
        .pipe(livereload()); // Adding livereload task here. Which creates a livereload server.
});
gulp.task('htmls', function () {
    gulp.src('public/*html')
        .pipe(livereload());
});
gulp.task('watch', function () {
    livereload.listen(); // Calling lister on livereload task, which will start listening for livereload client.
    gulp.watch('public/scripts/*.js', ['scripts']);
    gulp.watch('public/*.html', ['htmls']);
});
gulp.task('lint', function () {
    gulp.src('./**/*.js')
        .pipe(jshint());
});
gulp.task('nodejs', function () {
    livereload.listen(); // Calling lister on livereload task, which will start listening for livereload client.
    nodemon({script: 'app.js', ext: 'js', ignore: ['build/', 'public/']})
        .on('change', ['lint'])
        .on('restart', function () {
            console.log('restarted!')
        });
});
gulp.task('open', function () {
    var options = {
        url: 'http://localhost:3000'
    };
    gulp.src('./public/index.html')
        .pipe(open("", options));
});
gulp.task('default', ['scripts', 'watch', 'lint', 'nodejs', 'open']);
```

Follow Me
---
[Github](https://github.com/AmitThakkar)

[Twitter](https://twitter.com/amit_thakkar01)

[LinkedIn](https://in.linkedin.com/in/amitthakkar01)

[More Blogs By Me](https://amitthakkar.github.io/)