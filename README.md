# Docara plugin for Laravel Mix (based on Jigsaw)

[![MIT License](https://img.shields.io/github/license/tightenco/laravel-mix-jigsaw)](https://github.com/tightenco/laravel-mix-jigsaw/blob/master/LICENSE.md)
[![Latest Stable Version](https://img.shields.io/npm/v/laravel-mix-jigsaw)](https://www.npmjs.com/package/laravel-mix-jigsaw)
[![Total Downloads](https://img.shields.io/npm/dt/laravel-mix-jigsaw)](https://www.npmjs.com/package/laravel-mix-jigsaw)

`laravel-mix-docara` is a [Laravel Mix](https://github.com/JeffreyWay/laravel-mix) plugin for the Docara static site generator (based on [Jigsaw](https://github.com/tightenco/jigsaw)). It watches Docara files and triggers a Mix build when it detects changes.

```js
const mix = require('laravel-mix');
require('laravel-mix-docara');

mix.docara()
    .js('source/_assets/js/main.js', 'js')
    .css('source/_assets/css/main.css', 'css')
    .version();
```

## Installation

```sh
npm install -D https://github.com/simai/docara-mix
```

## Usage

Require the module in your `webpack.mix.js` file.

```js
const mix = require('laravel-mix');
require('laravel-mix-docara');
```

Enable the build tasks by calling `.docara()` anywhere in your Mix build chain.

```js
mix.js('source/_assets/js/main.js', 'js')
    .css('source/_assets/css/main.css', 'css')
    .docara();
```

By default this plugin watches common Docara file paths and triggers a new build when it detects changes. To add watched paths or override the watcher configuration, pass a config object to `.docara()`:

```js
// Add additional file paths to watch
mix.docara({
    watch: ['config.*.php'],
});

// Override the default config
mix.docara({
    watch: {
        files: ['source/posts/*.blade.php'],
        notDirs: ['source/_assets/', 'source/assets/', 'source/ignore/'],
    },
});
```

If you use [Laravel Mix's built-in BrowserSync integration](https://laravel-mix.com/docs/6.0/browsersync), you may need to configure it to watch Docara's paths:

```js
mix.docara()
    .browserSync({
        server: 'build_local',
        files: ['build_*/**'],
    });
```

## Caveats

- The plugin **cannot** detect the creation of new files immediately inside the `source/` directory of your site. If you create a new file like `source/home.blade.php`, you'll need to stop and restart Mix.
- With v1 of the plugin it was possible to add additional Webpack plugins/tasks that would run after the Docara build but before Mix finished compiling assets. This created compatibility issues between this plugin and Mix itself, and was removed in v2. If you need to perform additional processing after your Docara site is built, like minifying its HTML, you can do so using a Docara [event listener](https://doc.simai.io/en/event-listeners/).

## Credits

Huge thanks to [Brandon Nifong](https://github.com/Log1x) for creating the initial version of this plugin!

## License

Laravel Mix Docara is provided under the [MIT License](LICENSE.md).
