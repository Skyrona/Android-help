# 📱 Angular to Cordova
* Move curent directory to an angular project

## ⚙️ Initialization
* *Install cordova [@see cordova](https://git-scm.com/book/fr/v1/Les-bases-de-Git-D%C3%A9marrer-un-d%C3%A9p%C3%B4t-Git)*
```bash
npm install cordova --save-dev
```
Cordova need a configuration file, he can be generated with the cli `create` command when creating a projetc. You already have a project, so simply create this file.
* *Create a `config.xml` at the root of the angular project*
```xml
<?xml version='1.0' encoding='utf-8'?>
<widget id="com.angulartocordova.app" version="1.0.0" xmlns:android="http://schemas.android.com/apk/res/android">
    <name>AngularToCordova</name>
    <description>Angular to Cordova</description>
    <content src="index.html" />
    <access origin="*" />
    <allow-intent href="http://*/*" />
    <allow-intent href="https://*/*" />
    <allow-intent href="geo:*" />
    <preference name="DisallowOverscroll" value="true" />
    <preference name="android-minSdkVersion" value="14" />
    <preference name="orientation" value="portrait" />
    <engine name="android" spec="^6.3.0" />
</widget>
```

## 🎉 Convenience
* *Add cordova shortcut to your `package.json` per convenience*
```json
"scripts": {
    "cordova": "cordova",
```

## 📱 Platform
Cordova need to be in a cordova project for add a platform.
* *Create `www` directory*
```bash
mkdir www
```
* *Create`index.html` file*
```bash
touch www/index.html
```
You have to add a platform for the device build, each platform have is requirements. Refer to the targeted plateform [requirements](https://cordova.apache.org/docs/en/latest/guide/support/index.html):
* [@see android](https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html)
* [@see ios](https://cordova.apache.org/docs/en/latest/guide/platforms/ios/index.html)

For exemple per android [Gradle](https://gradle.org/install/) must be installed and SDK build tool licenses must be accepted at `ANDROID_HOME/tools/bin/sdkmanager --licenses`. A simple way to have the requirements is to install [Android Studio](https://developer.android.com/studio/)

* *Install a platform*
```bash
npm run cordova platform add android
```
Once the platform added, your mobile have to `enable Developer options`. Refer to your device documentation
* *[Enable Developer options](https://www.google.com/search?q=enable+Developer+options)*

## 🔌 Plugin
Your angular project need some authorizations, install some vital plugin for allow basic feature.
* *Install plugins*
```bash
npm run cordova plugin add cordova-plugin-device
npm run cordova plugin add cordova-plugin-statusbar
npm run cordova plugin add cordova-plugin-whitelist
npm run cordova plugin add cordova-plugin-geolocation
```
Check for plugins like [geolocation](https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-geolocation/index.html) and some of build-in .

## 🔨 Build
Cordova expect to build an app located in the `www` folder with as entry point an `index.html`. The `angular.json` file allow us to customize the build path for target the build output.
* *Edit the outputPath in `angular.json`*
```json
{
  "projects": {
    "architect": {
      "options": {
         "outputPath": "www",
```
The entry point have to provide the `content security policy`. Edit the `src/index.html` for add thee meta you need [@see CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP).
* *Edit the csp `src/index.html`*
```html
<meta http-equiv="Content-Security-Policy"
      content="default-src * 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; media-src * 'unsafe-inline';" />
```
You need to specify the base path
* *Edit the base in `src/index.html`*
```html
<base href="./">
```
Your app is ready for the build.
* *Build angular project*
```bash
npm run build
```
* *Deploy on device*
```bash
npm run cordova run android --device
```

## ❤️ Finally

Love your build