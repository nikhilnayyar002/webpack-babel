# Webpack - Babel Setup

This is near to base setup for any other project.
I only added react dependencies.

## Setup

### Install

* `git clone `
* `npm ci`

### Create folders & files

* create folder `src` folder and add `src/index.js` file.
  ```js
  import ReactDOM from "react-dom";
  
  function App() {
      return "Hello WORLD"
  }
  
  ReactDOM.render(
      <App />,
    document.getElementById("root")
  );
  ```
* create `public` folder. add `public/favicon.ico` (if you dont have one create a empty file and name it). add `public/index.html`
  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <title>Document</title>
  </head>
  <body>
      <div id="root"></div>
  </body>
  </html>
  ```
> Example branch: [react-min-src](https://github.com/nikhilnayyar002/react-min/tree/react-min-src). This repo also has all the [optional features](#optional-features) enabled.
## Commands

Commands | Inf
-|-
start | dev mode.
build | generate build files.
wm | run `wm-script.js` script with arguments. See [Optional Features](#optional-features).
speed-measure-dev | measure speed in dev mode.
commit | run **commitizen**. creates standard commit.
release | run **standard-version**. creates standard release.

## Optional Features

Enable/Disable optional features in the project. The feature setup is already added in the project but to enable/disable them you have to run commands. The commands actually install packages and set private variables in `wm-config.js` to keep track of features.

### typescript
* enable: 
  ```
  npm run wm -- enable-feat-typescript
  ```
  Replace `index.js` with `index.tsx` (default name defined in `wm-config.js`).
* disable: 
  ```
  npm run wm -- disable-feat-typescript
  ```
  Replace `index.tsx` with `index.js` (default name defined in `wm-config.js`).

### sass
Adds [sass](https://sass-lang.com/)
* enable: 
  ```
  npm run wm -- enable-feat-sass
  ```
* disable: 
  ```
  npm run wm -- disable-feat-sass
  ```
### Enable multiple features (in single command)
```
npm run wm -- enable-feat-typescript enable-feat-sass
```
### Update all features
This command updates features dependencies.
  ```
  npm run wm -- update-feat-all
  ```

## Updating the setup

You should generally update the setup after every release.

If you have made changes to any root files like `wm-config.js`, `webpack.dev.js` etc. that is you made custom changes. Then run the following command to review changes before overwriting your changes:

```
git fetch
git merge origin/master --no-ff --no-commit
```

Otherwise if you have not changed any of root files:

```
git pull origin master
```

Install the dependencies:

```
npm i
```

If you have installed any [optional features](#optional-features) then run [update](#update-all-features) command as well.

> Also you should avoid pulling `package-lock.json` if your dependencies in `package.json` are not exact as of **react-min/master**. Instead maintain your own  `package-lock.json`. Packages differ when you enable [optional features](#optional-features) that may install its own packages or user might install its own custom packages.

## Features

* add [env variables](https://create-react-app.dev/docs/adding-custom-environment-variables/) (in use in app only). Example:
  * add `HELLO = WORLD` in file say `.env`
  * Then in `wm-config.js`. Add `"HELLO"` for it to be available in your App.
    ```js
    const wmConfig = {
        ...
        environmentVariablesInApp: ["HELLO"]
    },
    ```
    In your App
    ```js
    console.log(process.env.HELLO)
    ```
  > env variables starting with `REACT_MIN_` are automatically added.
* add svg as component: Append `?react` after file path in import statement in order for this to work.
  ```js
  import MySvg from '@assets/react.svg?react';
  import mySvg from '@assets/react.svg';
  
  function App() {
      return (
       <>
         <MySvg width="200" height="200" viewBox="0 0 3500 3500" />
         <img src={mySvg} width="200" height="200" />
       </>
      )
  }
  ```
  > Second svg is not imported as component but will be resolved as string by webpack.
* import other files:
  ```js
  import sampleImg from '@assets/sample.jpeg';
  import sampleJson from '@assets/sample.json'; // { "hello" : "world" }
  import helloTxt from '@assets/hello.txt';

  console.log(sampleJson.hello)

  function App() {
      return (
       <>
         <pre>{helloTxt}</pre>
         <img src={sampleImg} width="200" height="200" />
       </>
      )
  }
  ```
* Directly edit webpack config as you like (`wm-*.js`, `webpack.*.js` files)
* edit [standard commit types](#commit-types) in `.czrc` file. This file is used by **commitizen**.
* edit `.versionrc.js` file used by **standard-version**.
* edit babel setup in `wm-config.js` (`browserslist`, `wmConfig.babel`)
* edit eslint (`.eslintrc.js`)
* update typescript (`tsconfig.json`). add custom modules types in `index.d.ts` file.


## Commit Guidelines

This repo use [commitizen](https://github.com/commitizen/cz-cli) & [standard-version](https://github.com/conventional-changelog/standard-version) for generating releases, tags & changelogs.

### Commit types

* ✨ Features (minor)(public): when you add functionality in a backwards compatible manner
* ⚡️ Performance (patch)(public): internal performance improvements
* 🛠️ Bug Fixes (patch)(public): when you make backwards compatible bug fixes. internal dependency updates "fix(deps)" etc
* 📝 Docs(public): project documentation updates
* ⛏️ Chore(private): Code Styling, Refactor, changes that are categorised as other and does not bring any version update.
* ☑️ Tests(private): add code to test your code

## Important Links
* [Webpack generating duplicate code to save number of requests](https://github.com/webpack/webpack/issues/13768)
* https://blog.logrocket.com/why-you-should-use-package-lock-json/

## devDependencies
- babel
  - core: "@babel/core"
  - general preset: "@babel/preset-env"
  - preset for react: "@babel/preset-react" 
- Eslint
  - core: "eslint"
  - Plugin
    - "eslint-plugin-react"
    - "eslint-plugin-react-hooks"
- Other Packages
  - "commitizen"
  - "cz-conventional-changelog"
  - "standard-version"
  - "dotenv-flow"
- Webpack
  - "webpack"
  - "webpack-cli"
  - "webpack-dev-server"
  - "webpack-merge"
  - Plugins
    - **important**
      - @pmmmwh/react-refresh-webpack-plugin
        - peer
          - "react-refresh"
      - "circular-dependency-plugin"
      - "css-minimizer-webpack-plugin"
      - "eslint-webpack-plugin"
      - "html-webpack-plugin"
      - "mini-css-extract-plugin"
      - "terser-webpack-plugin"
    - **not important**
      - "event-hooks-webpack-plugin"
      - "speed-measure-webpack-plugin"
      - "clean-terminal-webpack-plugin"
  - Loaders
    - "babel-loader"
    - "css-loader"
    - "style-loader"
    - "@svgr/webpack"