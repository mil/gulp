# Templating with YAML Front Matter and Swig
Templating, similar to systems found in blogging engines, can be setup using `gulp-swig`, `gulp-data`, and `gulp-front-matter` matter with ease.  Consider the example:

##### `page.html`

```html
---
title: My title
todos:
    - List item one
    - Second list item
    - Thrid list item
---
<html>
    <head>
        <title>{{ title }}</title>
    </head>
    <body>
        Things to do:
        <ul>{% for todo in todos %}
          <li>{{ todo }}</li>
        {% endfor %}</ul>
    </body>
</html>
```

##### `gulpfile.js`

```js
var gulp = require('gulp');
var swig = require('gulp-swig');
var data = require('gulp-data');
var fm   = require('gulp-front-matter');

gulp.task("compile-page", function() {
  gulp.src('page.html')
      .pipe(fm({
          property : 'vars',
          remove   : true
      }))
      .pipe(data(function(page) { 
          return page.vars;
      }))
      .pipe(swig())
      .pipe(gulp.dest('build'));
});

gulp.task("default", ["compile-page"]);
```
