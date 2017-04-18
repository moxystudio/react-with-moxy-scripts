# react-with-moxy-scripts

This package includes scripts and configuration used by [react-with-moxy](https://github.com/moxystudio/react-with-moxy).


## Installation

`$ npm install react-with-moxy-scripts --server-dev`


## Motivation

`react-with-moxy` started as a normal boilerplate with all the tooling built-in. While this gave full flexibility, it came with a lot of downsides:

- *That's not my code* feeling: a lot of code is required to build, serve and configure webpack
- A lot of devDependencies related to the tooling, which feels awkward
- Hard to update the tooling code since it's built in into the project


`react-with-moxy-scripts` abstracts all the complexity behind setting up webpack, server-side rendering and other complex tasks that would otherwise be contained in the top-level projects.


## Usage

In your package.json:

```json
{
    "scripts": {
        "start": "react-with-moxy-scripts start",
        "start:dev": "react-with-moxy-scripts start:dev -- watch",
        "build": "react-with-moxy-scripts build"
    }
}
```


### Customize webpack

Simply create a `webpack.config.js` file in the `config/` folder like so:

```js
function webpackConfig(config, rc) {
    // You may tweak config here, see: https://github.com/mozilla-neutrino/webpack-chain
}

module.exports = webpackConfig;
```

The `config` passed into the function is a [webpack-chain](https://github.com/mozilla-neutrino/webpack-chain) instance to easily allow you to modify the webpack configuration. You may look into [our config](TODO) to see what's being offered by default.


### Customize the server


Simply create a `server.js` file in the `config/` folder like so:

```js
function server(app) {
    // You may configure the express app, add routes or middleware
}
```

The `app` passed into the function is an instance of an [express](https://expressjs.com) app. This function runs before adding any routes or middleware so that you are able to force your own.


### Customize the folder structure and other aspects

`react-with-moxy` is opinionated in how a project should be structured - it expects the following structure:

```
config/
    config.js
    config-dev.js
    config-prod.js
    *webpack.config.js
    *server.js
src/
    pages/
    App.js
    App.css
    client-renderer.js
    server-renderer.js
web/
    .index.js
    favicon.icon
```


Though, you may change it by creating a `.rwm.config.js` file in the root of your project:

```js
module.exports = {
    buildDir: 'web/build',
    srcDir: 'src',
    configDir: 'config',
    webpackConfigFile: 'config/webpack.config.js',
    serverConfigFile: 'config/server.js',
}
```

Additionally you may tweak some other aspects. Note

```js
module.exports = {
    // Inject babel polyfills as part of the build (false by default, default setup uses https://polyfill.io)
    useBabelPolyfills: false,
    // Custom tester for async modules that will be available in the build manifest
    asyncModulesTest: (module) => /src\/pages\/([a-z][a-z-_/]+)\/index\.js$/.test(module),
}
```


## Tests

`$ npm test`   
`$ npm test:watch` during development


## License

[MIT License](http://opensource.org/licenses/MIT)
