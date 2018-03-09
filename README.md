`html-webpack-scripts-plugin` improves control over scripts generated by webpack [Webpack](https://webpack.js.org/).

## Introduction
[`html-webpack-plugin`](https://www.npmjs.com/package/html-webpack-plugin) will add scripts generated by [Webpack](https://webpack.js.org/) into generated HTML like `<script type="text/javascript" src="/bundle/vendor.0a78e31b5c440.js"></script>`  without need to include them manually. However it won't give you additional control. For example you can't set `defer` attribute or make them inline.
This plugin is similar to ['script-ext-html-webpack-plugin'](https://www.npmjs.com/package/script-ext-html-webpack-plugin), but may work in conjunction with ['html-webpack-template'](https://www.npmjs.com/package/html-webpack-template) which requires `inject: false`. 

Usage
----------------------

Assume you have two scripts generated by webpack: `vendor.0a78e31b5c440.js` `app.4234fe71c300ea.js`. Plugin gives you ability to:

#### Add specific attributes like `async` `defer` `id` `charset`:
```js
// webpack.config.js
plugins: [
  new HtmlWebpackScriptsPlugin({
    'defer charset=utf8': /vendor/,
    'async id=appscript': /app/
  })
]
```
Output:
```html
<script defer charset="utf8" type="text/javascript" src="vendor.0a78e31b5c440.js"></script>
<script async id="appscript" type="text/javascript" src="app.4234fe71c300ea.js"></script>
```

#### Add custom attributes like `data-*`
```js
// webpack.config.js
plugins: [
  new HtmlWebpackScriptsPlugin({
    'defer data-script-defer=true': /vendor/, 
    'charset=utf8 id=appscript inline data-script-inline=true': /app/
  })
]
```
Output:
```html
<script defer data-script-defer="true" type="text/javascript" src="vendor.0a78e31b5c440.js"></script>
<script charset="utf8" id="appscript" data-script-inline="true" type="text/javascript"> /* Content of app.4234fe71c300ea.js */ </script>
```

#### Make scripts inline:
```js
// vendor.0a78e31b5c440.js
var helloWebpack = 'hello webpack'
console.log(helloWebpack)
```
```js
// app.4234fe71c300ea.js
var helloApp = 'Hello app'
console.log(helloApp)
```
```js
// webpack.config.js
plugins: [
   new HtmlWebpackScriptsPlugin({ inline: /vendor|app/ })
]
```
Output:
```html
<script type="text/javascript">
var helloWebpack = 'hello webpack'
console.log(helloWebpack)
</script>

<script type="text/javascript">
var helloApp = 'Hello app'
console.log(helloApp)
</script>
```
