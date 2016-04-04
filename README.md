# MSL Pages

This repository is a sub-repository of the [Kenzan Million Song Library](https://github.com/kenzanmedia/million-song-library) (MSL) project, a microservices-based Web application built using [AngularJS](https://angularjs.org/), a [Cassandra](http://cassandra.apache.org/) NoSQL database, and [Netflix OSS](http://netflix.github.io/) tools.

> **NOTE:** For an overview of the Million Song Library microservices architecture, as well as step-by-step instructions for running the MSL demonstration, see the [Million Song Library Project Documentation](https://github.com/kenzanmedia/million-song-library/tree/develop/docs).

## Installation

The Million Song Library client/UI application was built using [Node 0.12.x](https://nodejs.org/en/download/) and [npm 2.7.x](https://nodejs.org/en/download/) or higher (see [NVM](https://github.com/creationix/nvm)). To install the client/UI, run the following command from the `/msl-pages` directory of the main [million-song-library](https://github.com/kenzanmedia/million-song-library/tree/develop/server) repository:

```
npm install
```

This will install `npm` dependencies as well as trigger `bower install` to install Bower dependencies.

> **NOTE:** If you receive an error when running this command, or any of the commands below, try using `sudo` (Mac and Linux) or run PowerShell as an administrator (Windows).

## Build

To build the client/UI, run the following command from the `/msl-pages` directory of the main [million-song-library](https://github.com/kenzanmedia/million-song-library/tree/develop/server) repository:

```
npm run build
```

This will create new source files and place them in `./build` directory. To use the source file, mount them on any server, or use the `npm run full-dev` command (see the [Dev Server](#Dev-Server) section below).


In order to run micro-service dependencies run the npm task `npm run build-server`. Optionally run `npm run build-and-serve` for
building dependencies and running an instance of Jetty for each micro-service

## Dev Server

Start dev server by calling `npm run full-dev`, this will automatically mount a server on `3000` localhost port.
If any change is made during dev server run, source files will be automatically rebuilt.

Add `msl.kenzanlabs.com` to your `/etc/hosts` file to map the localhost ip. This will include your localhost as a allowed
origin.

Install [asciidoctor](http://asciidoctor.org/) before running dev task:

*See API & Server section to see about running the API server.*

## Prod Server

Start prod server by calling `npm run serve-all`. This will try to mount `./build` directory to `80` port by
using [http-server](https://github.com/indexzero/http-server) for serving static resources. Before you run this make
sure you have called `npm run full-dev` first.

## ESDocs

Add command to generate ESDocs

From the `msl-pages` directory run the `gen-jsdocs` command, to generate documentation under `build/esdcos`

## Styleguide

To create a styleguide of the project run `npm run styleguide`. This will
generate styleguide's files using [Kss-node](https://github.com/kss-node/kss-node)
in `/build/styleguide/` directory.


### File Layout Structure

#### Source Files

Source files are stored in `./src` directory. When webpack start build it starts from `./src/index.js` file.
Library files from `npm` and `bower` can be accessed by simple imports like in `node.js`. We strongly suggest
to use `npm` dependencies and use `bower` for dependencies that are not published to `npm`.


 UPDATE:
 
```
├──  /src
│   ├── /layout
│   │   │   # overall site layout, navigation bar and components that are visible
│   │   │   # through out all views go here
│   ├── /modules
│   │   │   # place to put custom modules used all over the system
│   │   └── /<modules list...>
│   ├── /pages
│   │   │   # pages can have its controllers, factories, and other.
│   │   │   # Each page is a different angular module so it can
│   │   │   # have custom configuration and route. When using angular ui router you will
│   │   │   # define it states in page module route - not in the global one!
│   │   │   # this way you get a clear view on page dependencies.
│   │   └── /<pages list...>
│   │       ├── /controllers
│   │       ├── /factories
│   │       ├── /*-route.js
│   │       ├── /*-module.js
│   │       └── /*.html
│   ├── /styles
│   │   │   # place custom stylesheets used all over the system
│   │   └── /<stylesheet list...>
│   ├── /constants.js
│   ├── /routing.js
│   ├── /run.js
│   └── /index.js
```


### Testing

Project is tested with `karma` with `jasmine` framework and `eslint` for reporting on patterns. You can run tests and
pattern reporting by simply calling `npm run test`. Note that if `eslint` fails then tests will not be triggered.
You can also use `npm run autotest` for automated test runs when some source files changes.

#### Functional Testing

Project is tested with `jasmine` using `protractor` framework. First you need to install some global dependencies
(may need to use sudo).
- `npm install -g protractor`

- `npm install -g selenium-webdriver`

Now run the mock server `npm run serve-mock` and the site consuming that server `npm run dev -mock`.
Then `webdriver-manager start` (When running it for the first time, you may need to update the manager `webdriver-manager update --standalone`).
Finally run the tests by calling `protractor protractor.conf.js`. The tests will run in the different browsers(Chrome, Firefox, Safari and Internet Explorer).

#### Test Files

Test files are stored in `./test/specs` directory. When karma runner starts, it includes files by
`./test/specs/**/*.test.js` pattern only. Other files like source files that need to be tested have to be
imported in the test files.

## API & Server

In order to get API host add `process.env.API_HOST`. This will return host if it is defined at build time.

### Swagger

Swagger specification is separated into their corresponding micro-service specification for generating API REST code for each microservice
by using swagger-codegen tools.
In order to run an overall swagger spec the file is generated through runtime using the npm task `parse-swagger-src`. All process is part of the
npm tasks `build-server` and `serve-and-build`


#### Swagger Mock Server

Start mock server by running `npm run serve-mock`. **Note** that if you want that your dev server would
use swagger mock server you need to start it with `--mock` argument, for e.g. `npm run dev --mock`. Mock
server runs in localhost port 10010

#### Swagger Docs Editor

Start docs editor by running `npm run docs`.

#### Swagger UI

Runs on localhost port 10010/docs alongside swagger mock server when running it `npm run serve-mock`.

### Jersey API

for running microservices. Have to run build-and-serve first or build all microservices before running serve-all

To run the jersey api from the client directory use the npm task `serve-all` to a Jetty server instance for all microservices.
`build-and-serve` npm task builds server dependencies before running the Jetty instances.

