# Summary
Follow the links to read about the different topics.

- [Home](https://github.com/MaxHill/vue-starter/tree/master/docs)
- [Structure](https://github.com/MaxHill/vue-starter/tree/master/docs/structure.md)
- [Build Commands](https://github.com/MaxHill/vue-starter/tree/master/docs/commands.md)
- [Linter Configuration](https://github.com/MaxHill/vue-starter/tree/master/docs/linter.md)
- [Pre-processors](https://github.com/MaxHill/vue-starter/blob/master/docs/pre-processors.md)
- [Handle Static Assets](https://github.com/MaxHill/vue-starter/tree/master/docs/static.md)
- [Environment Variables](https://github.com/MaxHill/vue-starter/tree/master/docs/env.md)
- [Integrate with Backend Framework](https://github.com/MaxHill/vue-starter/tree/master/docs/backend.md)
- [API Proxying During Development](https://github.com/MaxHill/vue-starter/tree/master/docs/proxy.md)
- [Unit](https://github.com/MaxHill/vue-starter/tree/master/docs/unit.md)
- [E2e](https://github.com/MaxHill/vue-starter/tree/master/docs/e2e.md)
- [Prerender](https://github.com/MaxHill/vue-starter/tree/master/docs/prerender.md)


# Pre-Processors

This boilerplate has pre-configured CSS extraction for most popular CSS pre-processors including LESS, SASS, Stylus, and PostCSS. To use a pre-processor, all you need to do is installing the appropriate webpack loader for it. For example, to use SASS:

``` bash
npm install sass-loader node-sass --save-dev
```

Sass is already handled for you out of the box.

### Using Pre-Processors inside Components

Once installed, you can use the pre-processors inside your `*.vue` components using the `lang` attribute on `<style>` tags:

``` html
<style lang="scss">
/* write SASS! */
</style>
```

### A note on SASS syntax

- `lang="scss"` corresponds to the CSS-superset syntax (with curly braces and semicolones).
- `lang="sass"` corresponds to the indentation-based syntax.

### PostCSS

Styles in `*.vue` files are piped through PostCSS by default, so you don't need to use a specific loader for it. You can simply add PostCSS plugins you want to use in `build/webpack.base.conf.js` under the `vue` block:

``` js
// build/webpack.base.conf.js
module.exports = {
  // ...
  vue: {
    postcss: [/* your plugins */]
  }
}
```

See [vue-loader's related documentation](http://vuejs.github.io/vue-loader/features/postcss.html) for more details.

### Standalone CSS Files

To ensure consistent extraction and processing, it is recommended to import global, standalone style files from your root `App.vue` component, for example:

``` html
<!-- App.vue -->
<style src="./styles/global.less" lang="less"></style>
```

Note you should probably only do this for the styles written by yourself for your application. For existing libraries e.g. Bootstrap or Semantic UI, you can place them inside `/static` and reference them directly in `index.html`. This avoids extra build time and also is better for browser caching. (See [Static Asset Handling](static.md))
