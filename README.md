Configurable gulp-based assets builder
======================================

A small collection of gulp tasks we use to build front-end code, for fully static or CMS-based website projects. Includes Sass, Autoprefixer, SVG sprites and JS minification.


Highlights
----------

-   Flexible configuration, in a separate file
-   Each build task is only active if the corresponding config is set
-   System notifications for errors (using [node-notifier][])
-   Logs information on output files (using [gulp-size][])


How to use this repo
--------------------

This is an example for using [gulp][], not a library. We recommend [downloading a zip of this repo][ZIP] and using it as a starting point for a project, changing the directory structure or the JS code as you see fit.

Console usage uses npm scripts:

```
# Install dependencies (requires Node.js)
npm install

# Build CSS/JS/SVG once
npm run build

# Build CSS/JS/SVG on changes
npm run watch

# (Optional) check that tasks are correctly configured
npm run list
```


Changing the location of the gulp scripts
-----------------------------------------

-   If you rename the `assets/config.js` file or move it somewhere else, update the `assets` property in the `"config"` section of `package.json`.

-   If you rename the `gulp` directory or move it somewhere else, update the `--gulpfile` parameters in the `"scripts"` section of `package.json`.


Configuring tasks
-----------------

Configuration is in `assets/config.js` (by default). Config objects have the following properties:

-   Required `src`: paths or glob patterns (string or array of strings).
-   Required `dest`: a single path (string).
-   Optional `watch`: either the value `true` to reuse the `src` config, or a different set of paths or glob patterns (string or array of strings).

Some tasks may offer additional options. See the next section for details.

### Sass → Autoprefixer → CSS

Config key: `sass`<br>
Task script: `gulp/builders/sass.js`

```js
{
  // (Required) Source can be a single Sass stylesheet
  // or pattern, or an array of paths or patterns
  src: 'assets/styles/*.scss',
  // (Required) Destination must be a folder
  dest: 'public/css',
  // (Optional) File patterns to watch for changes
  watch: 'assets/styles/**/*.scss',
  // (Optional) Autoprefixer: target browsers
  browsers: ['last 3 versions']
}
```

### JS scripts → Concatenated and minified JS

Config key: `jsconcat`<br>
Task script: `gulp/builders/jsconcat.js`

```js
{
  // (Required) Source files. Like with other tasks, we can
  // provide an array of paths instead of a single path.
  src: [ 'assets/libs/hello.js',
         'assets/scripts/*.js' ],
  // (Required) Destination must be a file name
  dest:  'public/js/main.js',
  // (Optional) File patterns to watch for changes, or `true`
  // to use the same value as the src property
  watch: true
}
```

### SVG images → [SVG symbol sprites][SVG_SPRITES]

Config key: `svgsprite`<br>
Task script: `gulp/builders/svgsprite.js`

```js
{
  // (Required) Source SVG images
  src: 'assets/icons/main/*.svg',
  // (Required) Destination must be a file name
  dest: 'public/svg/main.svg',
  // (Optional) File patterns to watch for changes, or `true`
  // to use the same value as the src property
  watch: true,
}
```

### More than one build per task?

Each task can accept several config objects, to output several result files. You need to provide a full config object for each `dest` file.

```js
{
  …,
  jsconcat: [
    { src:  'assets/scripts/topic1/*.js',
      dest: 'public/svg/main.svg',
      watch: true
    },
    { src:  'assets/scripts/topic2/*.js',
      dest: 'public/svg/main.svg'
    }
  ],
  …
}
```


[gulp]: http://gulpjs.com/
[gulp-size]: https://www.npmjs.com/package/gulp-size
[node-notifier]: https://www.npmjs.com/package/node-notifier
[SVG_SPRITES]: http://fvsch.com/code/svg-icons/how-to/
[ZIP]: releases/latest