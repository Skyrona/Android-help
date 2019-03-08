# 🐣 Setting-up Angular 7
* Install [NodeJS](https://nodejs.org/en/download/) with [npm](https://www.npmjs.com/) then open a terminal
## ⚙️ Initialization
A project creation is well documented [@see getting started](https://angular.io/guide/quickstart).
* Install @angular/cli
```bash
npm install @angular/cli
```
*  Create a project and maybe look for options [@see new](https://angular.io/cli/new)
```bash
node_modules/.bin/ng new my-project --style scss
```
* Move the current directory to the project
```bash
cd my-project
```
* Serve the project
```bash
npm run start
```
## 💙 Angular material
Install the MDC with ng add [@see add](https://angular.io/cli/add) and [@see material.angular](https://material.angular.io/guide/getting-started).
```bash
npm run ng add @angular/material
```
The after hook will make changes to include the library, maybe you want to fix them per exemple `Hammer.JS` is well embed in the `main.js`.
### 📄 angular.json
Default template is linked in `angular.json` on the `styles` array for `build` and `test` properties, find "styles" occurence and remove links to the template. Why? Because if you use scss style it's more convenient to embeed dependencies in `styles.scss`. 
* Add template dependency to styles.scss
```scss
@import "~@angular/material/prebuilt-themes/indigo-pink.css";
```
### 📄 index.html
`Roboto` and the `material design icons` are linked to the `index.html`. Maybe you want to embeed dependencies if network is off of for hybrid build. Remove links to roboto and material design icons in the index.html.
* Install roboto
```bash
npm i roboto-fontface
```
* Add roboto dependency to style.scss
```scss
@import "~roboto-fontface/css/roboto/roboto-fontface.css";
```
* Install  material-design-icons
```bash
npm i material-design-icons
```
* Add  material-design-icons dependency to style.scss
```scss
@import "~material-design-icons/iconfont/material-icons.css";

```
For MDC intégration, a good practice is to create a module responsible to imports and exports material modules, this setup do not delivre usage guideline.
## 🧡 Flex layout
An ecosystem-friendly library for responsive flex design can be embedded.
* Install flexlayout
```bash
npm i @angular/flex-layout
```
* Embed FlexLayoutModule to your shared module
```js
import {FlexLayoutModule} from "@angular/flex-layout";
```
## 🐥 Usage

The framework is ready to use [@see angular.io](https://angular.io/)
```bash
npm run start
```