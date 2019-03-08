# 🐣 Setting-up Sass

* Install [NodeJS](https://nodejs.org/en/download/) with [npm](https://www.npmjs.com/) then open a terminal


## ⚙️ Initialization
Move current directory to a project
* *Initialize project*
```bash
npm init
```
We going to install `webpack` module bundler then embed `sass` preprocessor and `browser-sync` hot reloader for convenience

* *Install packages*
```bash
npm install browser-sync browser-sync-webpack-plugin css-loader file-loader mini-css-extract-plugin node-sass sass-loader  style-loader webpack webpack-cli --save-dev
```

## 💙 WebPack 
We need to extend `WebPack` default configuration configuration, we can specify a file configuration at the root of the project. We going to specify entry points for Sass files then the output dir. We gonna specify loaders then configure plugin's like `BrowserSyncPlugin` for the development server
* *Create a `webpack.config.js` file*
```js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const BrowserSyncPlugin = require('browser-sync-webpack-plugin');

module.exports = {
    entry: [
        './assets/sass/index.scss'
    ],
    output: {
        path: __dirname + "/dist",
        filename: 'index.js'
    },
    module: {
        rules: [
            {
                test: /\.scss$/,
                exclude: /node_modules/,
                use: [
                    MiniCssExtractPlugin.loader,
                    'css-loader',
                    'sass-loader'
                ]
            },
            {
                test: /\.(jpg|png|gif|woff|woff2|eot|ttf|svg)$/,
                loader: 'file-loader'
            }
        ]
    },
    watchOptions: {
        ignored: [
            /node_modules/,
        ]
    },
    plugins: [
        new MiniCssExtractPlugin({
            filename: 'index.css',
        }),
        new BrowserSyncPlugin({
            
            files: [
                'index.html',
                'assets/**/*.css'
            ],
            host: 'localhost',
            port: 3000,
            server: {
                baseDir: ['.']
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

* *Create `index.scss` in assets/sass folder*
* *Create `index.html` at the root of the project directory*

The configuration expect you have folowing file structure, adapt as needed
```bash
|_ dist
|_ assets
   |_ sass
        |_ index.scss
|_ index.html
|_ package.json
|_ webpack.config.js
```
* *Embed bundled style and script in the `index.html`*
```html
<link rel="stylesheet" type="text/css" href="dist/index.css" />
```

## 🐥 Usage

The stack is ready to use, start to dev by running following command. Styles are bundled in dist folder when you save for changes
```bash
npm run start
```
