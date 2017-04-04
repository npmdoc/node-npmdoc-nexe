# api documentation for  [nexe (v1.1.2)](https://github.com/jaredallard/nexe#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-nexe.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-nexe) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-nexe.svg)](https://travis-ci.org/npmdoc/node-npmdoc-nexe)
#### create single executables out of your [node/io].js applications

[![NPM](https://nodei.co/npm/nexe.png?downloads=true)](https://www.npmjs.com/package/nexe)

[![apidoc](https://npmdoc.github.io/node-npmdoc-nexe/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-nexe_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-nexe/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-nexe/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-nexe/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Jared Allard",
        "email": "jaredallard@outlook.com"
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
            "email": "craig.j.condon@gmail.com",
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
            "name": "architectd",
            "email": "craig.j.condon@gmail.com"
        },
        {
            "name": "crcn",
            "email": "craig.j.condon@gmail.com"
        },
        {
            "name": "rainbowdashdc",
            "email": "jaredallard@outlook.com"
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
    "readme": "ERROR: No README data found!",
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



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module nexe](#apidoc.module.nexe)
1.  [function <span class="apidocSignatureSpan">nexe.</span>compile (options, complete)](#apidoc.element.nexe.compile)
1.  [function <span class="apidocSignatureSpan">nexe.</span>package (path, options)](#apidoc.element.nexe.package)



# <a name="apidoc.module.nexe"></a>[module nexe](#apidoc.module.nexe)

#### <a name="apidoc.element.nexe.compile"></a>[function <span class="apidocSignatureSpan">nexe.</span>compile (options, complete)](#apidoc.element.nexe.compile)
- description and source-code
```javascript
compile = function (options, complete) {

  var nodeCompiler, nexeEntryPath;

  async.waterfall([
<span class="apidocCodeCommentSpan">    /**
     *check relevant options
     */
</span>    function checkOpts(next) {
      /* failsafe */
      if (options === undefined) {
        _log("error", "no options given to .compile()");
        process.exit()
      }

      /**
       * Have we been given a custom flag for python executable?
       **/
      if (options.python !== 'python' && options.python !== "" && options.python !== undefined) {
        if (isWin) {
          isPy = options.python.replace(/\//gm, "\\"); // use windows file paths, batch is sensitive.
        } else {
          isPy = options.python;
        }

        _log("set python as " + isPy);
      } else {
        isPy = "python";
      }

      // remove dots
      options.framework = options.framework.replace(/\./g, "");

      // set outter-scope framework variable.
      framework = options.framework;
      _log("framework => " + framework);

      version = options.nodeVersion; // better framework vc

      // check iojs version
      if (framework === "iojs" && version === "latest") {
        _log("fetching iojs versions");
        mkdirp(options.nodeTempDir); // make temp dir, probably repetive.

        // create write stream so we have control over events
        var output = fs.createWriteStream(path.join(options.nodeTempDir,
          "iojs-versions.json"));

        request.get("https://iojs.org/dist/index.json")
          .pipe(output);

        output.on('close', function() {
          _log("done");
          var f = fs.readFileSync(path.join(options.nodeTempDir,
            "iojs-versions.json"));
          f = JSON.parse(f);
          version = f[0].version.replace("v", "");

          _log("iojs latest => " + version);

          // continue down along the async road
          next();
        });
      } else {
        next();
      }
    },

    /**
     * first download node
     */
    function downloadNode(next) {
      _downloadNode(version, options.nodeTempDir, options.nodeConfigureArgs, options.nodeMakeArgs, next);
    },

    /**
     * Embed Resources into a base64 encoded array.
     **/
    function embedResources(nc, next) {
      nodeCompiler = nc;

      _log("embedResources %s", options.resourceFiles);
      embed(options.resourceFiles, options.resourceRoot, nc, next);
    },

    /**
     * Write nexeres.js
     **/
    function writeResources(resources, next) {
      let resourcePath = path.join(nodeCompiler.dir, "lib", "nexeres.js");
      _log("resource -> %s", resourcePath);

      fs.writeFile(resourcePath, resources, next);
    },

    /**
     * Bundle the application into one script
     **/
    function combineProject(next) {
      _log("bundle %s", options.input);
      bundle(options.input, nodeCompiler.dir, options, next);
    },

    /**
     * monkeypatch some files so that the nexe.js file is loaded when the app runs
     */

    function monkeyPatchNodeConfig(next) {
      _monkeyPatchNodeConfig(nodeCompiler, next);
    },

    /**
     * monkeypatch node.cc to prevent v8 and node from processing CLI flags
     */
    function monkeyPatchNodeCc(next) {
      if (options.flags) {
        _monkeyPatchMainCc(nodeCompiler, next);
      } else {
        next();
      }
    },

    function monkeyPatchv8FlagsCc(next) {
      if (options.jsFlags) {
        return _monkeyPatchv8FlagsCc(nodeCompiler, options, next);
      }

      return next();
    },

    /**
     * monkeypatch child_process.js so nexejs knows when it is a forked process
     */
    function monkeyPatchChildProc(next) {
      _monkeyPatchChildProcess(nodeCompiler, next);
    },

    /**
     * If an old compiled executable exists in the Release directory, delete it.
     * This lets us see if the build failed by checking the existence of this file later.
     */

    function cleanUpOldExecutable(next) {
      fs.unlink(nodeCompiler.releasePath, function(err) {
        if (err) {
          if (err.code === "ENOENT") {
            next();
          } else {
            throw err;
          } ...
```
- example usage
```shell
...

### Code Usage

'''javascript

var nexe = require('nexe');

nexe.compile({
	input: 'input.js', // where the input file is
	output: 'path/to/bin', // where to output the compiled binary
	nodeVersion: '5.5.0', // node version
	nodeTempDir: 'src', // where to store node source.
	nodeConfigureArgs: ['opt', 'val'], // for all your configure arg needs.
	nodeMakeArgs: ["-j", "4"], // when you want to control the make process.
	python: 'path/to/python', // for non-standard python setups. Or python 3.x forced ones.
...
```

#### <a name="apidoc.element.nexe.package"></a>[function <span class="apidocSignatureSpan">nexe.</span>package (path, options)](#apidoc.element.nexe.package)
- description and source-code
```javascript
package = function (path, options) {
  let _package; // scope

  // check if the file exists
  if (fs.existsSync(path) === false) {
    _log("warn", "no package.json found.");
  } else {
    _package = require(path);
  }

  if(!_package || !_package.nexe) {
    _log('error', 'trying to use package.json variables, but not setup to do so!');
    process.exit(1);
  }

  // replace ^$ w/ os specific extension on output
  if (isWin) {
    _package.nexe.output = _package.nexe.output.replace(/\^\$/, '.exe') // exe
  } else {
    _package.nexe.output = _package.nexe.output.replace(/\^\$/, '') // none
  }

  // construct the object
  let obj = {
    input: (_package.nexe.input || options.i),
    output: (_package.nexe.output || options.o),
    flags: (_package.nexe.runtime.ignoreFlags || (options.f || false)),
    nodeMakeArgs: (_package.nexe.runtime.nodeMakeArgs || []),
    resourceFiles: (_package.nexe.resourceFiles),
    nodeVersion: (_package.nexe.runtime.version || options.r),
    nodeConfigureArgs: (_package.nexe.runtime.nodeConfigureArgs || []),
    nodeMakeArgs: (_package.nexe.runtime.nodeMakeArgs || []),
    jsFlags: (_package.nexe.runtime['js-flags'] || options.j),
    python: (_package.nexe.python || options.p),
    debug: (_package.nexe.debug || options.d),
    nodeTempDir: (_package.nexe.temp || options.t),
    framework: (_package.nexe.runtime.framework || options.f)
  }

  // browserify options
  if(_package.nexe.browserify !== undefined) {
    obj.browserifyRequires = (_package.nexe.browserify.requires || []);
    obj.browserifyExcludes = (_package.nexe.browserify.excludes || []);
    obj.browserifyPaths    = (_package.nexe.browserify.paths    || []);
  }

  // TODO: get rid of this crappy code I wrote and make it less painful to read.
  Object.keys(_package.nexe).forEach(function(v, i) {
    if (v !== "runtime" && v !== 'browserify') {
      _log("log", v + " => '" + _package.nexe[v] + "'");
    }
  });

  return obj;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
