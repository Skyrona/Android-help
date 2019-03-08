# 🐣 Setting-up ES6

* Install [NodeJS](https://nodejs.org/en/download/) with [npm](https://www.npmjs.com/) then open a terminal


## ⚙️ Initialization
Move current directory to a project
* *Initialize project*
```bash
npm init
```
We going to install `babel` transpiler, configure it for `webpack` module bundler then embed `sass` preprocessor and `browser-sync` hot reloader for convenience


* *Install packages*
```bash
npm install @babel/cli @babel/core @babel/preset-env @babel/register babel-loader browser-sync browser-sync-webpack-plugin css-loader mini-css-extract-plugin node-sass sass-loader style-loader webpack webpack-cli --save-dev
```

## 💛 Babel 
`Babel` need a configuration, we can specify a file-relative configuration at the root of the project directory
* *Create a `.babelrc` file*
```js
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

## 💙 WebPack 
We need to extend `WebPack` default configuration configuration, we can specify a file configuration at the root of the project. We going to specify entry points for JavaScript and Sass files then the output dir. We gonna specify loaders then configure plugin's like `BrowserSyncPlugin` for the development server
* *Create a `webpack.config.js` file*
```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const BrowserSyncPlugin = require('browser-sync-webpack-plugin');

module.exports = {
    entry: [
        './src/index.js',
        './src/index.scss'
    ],
    output: {
        path: __dirname + "/dist",
        filename: 'index.js'
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: [
                    'babel-loader'
                ]
            },
            {
                test: /\.scss$/,
                exclude: /node_modules/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    'sass-loader'
                ]
            }
        ]
    },
    watchOptions: {
        ignored: [
            /node_modules/,
            /test/
        ]
    },
    plugins: [
        new MiniCssExtractPlugin(
            {
                filename: 'index.css',
            }
        ),
        new BrowserSyncPlugin({
            host: 'localhost',
            port: 3000,
            files: [
                'index.html',
            ],
            server: {
                baseDir: [
                    './'
                ]
            }
        })
    ]
};
```
## ❤️ Package

We can specify scripts in the package.json file as shortcut to webpack cli instructions in the scripts attribute

* *Add scripts in the `package.json`*
```json
  "scripts": {
    "start": "npm run dev",
    "dev": "webpack --mode development --watch",
    "prod": "webpack --mode production --watch",
    "build": "webpack --mode production"
  }
```

## 💜 Files

* *Create `index.js` in src folder*
* *Create `index.scss` in src folder*
* *Create `index.html` at the root of the project directory*

The configuration expect you have folowing file structure, adapt as needed
```bash
|_ dist
|_ src
   |_ index.js
   |_ index.scss
|_ .babelrc
|_ index.html
|_ package.json
|_ package.json
|_ webpack.config.js
```
* *Embed bundled style and script in the `index.html`*
```html
<link rel="stylesheet" type="text/css" href="dist/index.css" />
```
```html
<script type="text/javascript" src="dist/index.js"></script>
```

## 🐥 Usage

The stack is ready to use, start to dev by running following command. Scripts are bundled in dist folder when you save for changes then you can use safely [es6 syntax](https://developer.mozilla.org/fr/docs/Web/JavaScript/Nouveaut%C3%A9s_et_historique_de_JavaScript/Support_ECMAScript_2015_par_Mozilla)
```bash
npm run start
```
