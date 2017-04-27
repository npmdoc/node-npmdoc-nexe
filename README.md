# npmdoc-nexe

#### basic api documentation for  [nexe (v1.1.2)](https://github.com/jaredallard/nexe#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-nexe.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-nexe) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-nexe.svg)](https://travis-ci.org/npmdoc/node-npmdoc-nexe)

#### create single executables out of your [node/io].js applications

[![NPM](https://nodei.co/npm/nexe.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/nexe)

- [https://npmdoc.github.io/node-npmdoc-nexe/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-nexe/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-nexe/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-nexe/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-nexe/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-nexe/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Jared Allard"
    },
    "bin": {
        "nexe": "bin/nexe"
    },
    "bugs": {
        "url": "https://github.com/jaredallard/nexe/issues"
    },
    "contributors": [
        {
            "name": "Criag Condon",
            "url": "http://crcn.io/"
        }
    ],
    "dependencies": {
        "async": "^1.5.2",
        "browserify": "^13.0.0",
        "colors": "^1.1.2",
        "glob": "^7.0.0",
        "gunzip-maybe": "^1.3.1",
        "insert-module-globals": "^7.0.1",
        "mkdirp": "^0.5.1",
        "module-deps": "^4.0.5",
        "ncp": "^2.0.0",
        "progress": "^1.1.8",
        "request": "^2.67.0",
        "tar-stream": "^1.3.1",
        "yargs": "^4.2.0"
    },
    "description": "create single executables out of your [node/io].js applications",
    "devDependencies": {
        "mocha": "^2.4.5",
        "mocha-circleci-reporter": "0.0.1"
    },
    "directories": {},
    "dist": {
        "shasum": "3f9e8129456cba1abec9d153619446cde5599bad",
        "tarball": "https://registry.npmjs.org/nexe/-/nexe-1.1.2.tgz"
    },
    "gitHead": "64730f219f462ebdd476ff32f15dd05017ae5c50",
    "homepage": "https://github.com/jaredallard/nexe#readme",
    "license": "MIT",
    "main": "./lib/index.js",
    "maintainers": [
        {
            "name": "architectd"
        },
        {
            "name": "crcn"
        },
        {
            "name": "rainbowdashdc"
        }
    ],
    "name": "nexe",
    "nexe": {
        "input": "./bin/nexe",
        "output": "nexe^$",
        "temp": "src",
        "runtime": {
            "framework": "nodejs",
            "version": "5.5.0",
            "ignoreFlags": true
        }
    },
    "optionalDependencies": {},
    "preferGlobal": true,
    "repository": {
        "type": "git",
        "url": "git://github.com/jaredallard/nexe.git"
    },
    "scripts": {
        "test": "echo node: $(node -v) && node_modules/.bin/mocha --reporter mocha-circleci-reporter test/test.js"
    },
    "version": "1.1.2"
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
