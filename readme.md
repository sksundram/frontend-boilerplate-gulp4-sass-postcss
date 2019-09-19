# Instructions

In your terminal, run `gulp`.

---

1. `sudo npm -g gulp-cli` [verify installation: `gulp --version`]

2. `md frontend-boilerplate-gulp4-sass-postcss`

3. `cd frontend-boilerplate-gulp4-sass-postcss`

4. `npm init -y`

5. `touch index.html`

6. Fill `index.html` with boilerplate code

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Frontend boilerplate</title>
    <!-- CSS File -->
    <link rel="stylesheet" href="dist/style.css?cb=123" />
  </head>
  <body>
    <h1>Frontend boilerplate with Gulp 4, Sass and PostCSS</h1>

    <!-- JS File -->
    <script src="dist/all.js?cb=123"></script>
  </body>
</html>
```

7. `yarn add gulp gulp-sass gulp-sourcemaps gulp-postcss autoprefixer cssnano gulp-concat gulp-uglify gulp-replace -D`

8. `touch gulpfile.js`

9. In `gulpfile.js`, type:

```js
// Initialize modules
const { src, dest, watch, series, parallel } = require('gulp');
const autoprefixer = require('autoprefixer');
const cssnano = require('cssnano');
const concat = require('gulp-concat');
const postcss = require('gulp-postcss');
const replace = require('gulp-replace');
const sass = require('gulp-sass');
const sourcemaps = require('gulp-sourcemaps');
const uglify = require('gulp-uglify');

// File path variables
const files = {
  scssPath: 'app/scss/**/*.scss',
  jsPath: 'app/js/**/*.js'
};

// Sass task
function scssTask() {
  return src(files.scssPath)
    .pipe(sourcemaps.init())
    .pipe(sass())
    .pipe(postcss([autoprefixer(), cssnano()]))
    .pipe(sourcemaps.write('.'))
    .pipe(dest('dist'));
}

// JS task
function jsTask() {
  return src(files.jsPath)
    .pipe(concat('all.js'))
    .pipe(uglify())
    .pipe(dest('dist'));
}

// Cachebusting task
const cbString = new Date().getTime();
function cacheBustTask() {
  return src(['index.html'])
    .pipe(replace(/cb=\d+/g, 'cb=' + cbString))
    .pipe(dest('.'));
}

// Watch task
function watchTask() {
  watch([files.scssPath, files.jsPath], parallel(scssTask, jsTask));
}

// Default task
exports.default = series(parallel(scssTask, jsTask), cacheBustTask, watchTask);
```

10. Folder structure

```bash
.
├── app
│   ├── js
│   │   └── script.js
│   └── scss
│       └── style.scss
├── gulpfile.js
├── index.html
├── package.json
├── readme.md
└── yarn.lock
```

11. In your terminal, run `gulp`. A `dist` folder will be created and gulp will watch for any file changes.
