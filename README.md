# react-with-moxy-scripts

This package includes scripts and configuration used by [react-with-moxy](https://github.com/moxystudio/react-with-moxy).


## Installation

`$ npm install react-with-moxy-scripts --save-dev`


## Motivation

`react-with-moxy` started as a normal boilerplate with all the tooling built-in. While this gave full flexibility, it came with a lot of downsides:

- *That's not my code* feeling: a lot of code is required to build, serve and configure webpack
- A lot of devDependencies related to the tooling, which feels awkward
- Hard to update the tooling code since it's built in into the project


`react-with-moxy-scripts` abstracts all the complexity behind setting up webpack, server-side rendering and other complex tasks that would otherwise be part of top-level projects.


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
function webpack(config, { type, env, minify, rc }) {
    // You may tweak the webpack `config`
    // Additionally:
    // - `type` is either client or server
    // - `env` is the environment being configured
    // - `minify` is a flag telling us to optimize the assert or not
    // - `rc` is the runtime configuration, see bellow
}

module.exports = webpack;
```

The `config` passed into the function is a [webpack-chain](https://github.com/mozilla-neutrino/webpack-chain) instance to easily allow you to modify the webpack configuration. You may look into [our config](TODO) to see what's being offered by default.


### Customize the server


Simply create a `server.js` file in the `config/` folder like so:

```js
function server(app) {
    // `app` is an express server instance
    // You made configure it, add routes or add middleware
}
```

The `app` passed into the function is an instance of an [express](https://expressjs.com) app. This function runs before adding any routes or middleware so that you are able to force your own.


### Customize the folder structure and other aspects

`react-with-moxy` is opinionated in how a project should be structured - it expects the following structure:

```
*config/
    config.js
    config-dev.js
    config-prod.js
    *webpack.js (optional)
    *server.js (optional)
src/
    pages/
    App.js
    App.css
    *client-renderer.js
    *server-renderer.js
web/
    *build/
    .index.js
    favicon.icon
```

*Paths signaled with * are enforced* but you may change their location by creating a `.rwm.config.js` file in the root of your project:

```js
module.exports = {
    configDir: 'config',
    webpackConfigFile: 'config/webpack.js',
    serverConfigFile: 'config/server.js',
    clientRendererFile: 'src/client-renderer.js',
    serverRendererFile: 'src/server-renderer.js',
    buildDir: 'web/build',
};
```

Additionally you may tweak some other advanced aspects:

```js
module.exports = {
    // Inject `babel-polyfill` as part of the build
    // This is disabled by default, standard setup uses https://polyfill.io
    useBabelPolyfill: true,
    // TODO:
};
```


## Tests

`$ npm test`   
`$ npm test:watch` during development


## License

[MIT License](http://opensource.org/licenses/MIT)
