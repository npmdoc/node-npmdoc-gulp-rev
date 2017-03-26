# api documentation for  [gulp-rev (v7.1.2)](https://github.com/sindresorhus/gulp-rev#readme)  [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-rev.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-rev)
#### Static asset revisioning by appending content hash to filenames: unicorn.css => unicorn-d41d8cd98f.css

[![NPM](https://nodei.co/npm/gulp-rev.png?downloads=true)](https://www.npmjs.com/package/gulp-rev)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-rev/build/screen-capture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-rev_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-rev/build..beta..travis-ci.org/apidoc.html)

![package-listing](https://npmdoc.github.io/node-npmdoc-gulp-rev/build/screen-capture.npmPackageListing.svg)



# package.json

```json

{
    "author": {
        "name": "Sindre Sorhus",
        "email": "sindresorhus@gmail.com",
        "url": "sindresorhus.com"
    },
    "bugs": {
        "url": "https://github.com/sindresorhus/gulp-rev/issues"
    },
    "dependencies": {
        "gulp-util": "^3.0.0",
        "modify-filename": "^1.1.0",
        "object-assign": "^4.0.1",
        "rev-hash": "^1.0.0",
        "rev-path": "^1.0.0",
        "sort-keys": "^1.0.0",
        "through2": "^2.0.0",
        "vinyl-file": "^1.1.0"
    },
    "description": "Static asset revisioning by appending content hash to filenames: unicorn.css => unicorn-d41d8cd98f.css",
    "devDependencies": {
        "mocha": "*",
        "xo": "*"
    },
    "directories": {},
    "dist": {
        "shasum": "5e17cc229f6b45c74256f88ad3f2d3e9a3305829",
        "tarball": "https://registry.npmjs.org/gulp-rev/-/gulp-rev-7.1.2.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "files": [
        "index.js"
    ],
    "gitHead": "e895123b92f4a39fcf6a3f37dbdd78bc6f93c0c5",
    "homepage": "https://github.com/sindresorhus/gulp-rev#readme",
    "keywords": [
        "gulpplugin",
        "rev",
        "revving",
        "revision",
        "hash",
        "optimize",
        "version",
        "versioning",
        "cache",
        "expire",
        "static",
        "asset",
        "assets"
    ],
    "license": "MIT",
    "maintainers": [
        {
            "name": "sindresorhus",
            "email": "sindresorhus@gmail.com"
        },
        {
            "name": "bobthecow",
            "email": "npm@0x7f.us"
        }
    ],
    "name": "gulp-rev",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/sindresorhus/gulp-rev.git"
    },
    "scripts": {
        "test": "xo && mocha"
    },
    "version": "7.1.2",
    "xo": {
        "envs": [
            "node",
            "mocha"
        ]
    }
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-rev](#apidoc.module.gulp-rev)
1.  [function <span class="apidocSignatureSpan">gulp-rev.</span>manifest (pth, opts)](#apidoc.element.gulp-rev.manifest)



# <a name="apidoc.module.gulp-rev"></a>[module gulp-rev](#apidoc.module.gulp-rev)

#### <a name="apidoc.element.gulp-rev.manifest"></a>[function <span class="apidocSignatureSpan">gulp-rev.</span>manifest (pth, opts)](#apidoc.element.gulp-rev.manifest)
- description and source-code
```javascript
manifest = function (pth, opts) {
	if (typeof pth === 'string') {
		pth = {path: pth};
	}

	opts = objectAssign({
		path: 'rev-manifest.json',
		merge: false,
		// Apply the default JSON transformer.
		// The user can pass in his on transformer if he wants. The only requirement is that it should
		// support 'parse' and 'stringify' methods.
		transformer: JSON
	}, opts, pth);

	var manifest = {};

	return through.obj(function (file, enc, cb) {
		// ignore all non-rev'd files
		if (!file.path || !file.revOrigPath) {
			cb();
			return;
		}

		var revisionedFile = relPath(file.base, file.path);
		var originalFile = path.join(path.dirname(revisionedFile), path.basename(file.revOrigPath)).replace(/\\/g, '/');

		manifest[originalFile] = revisionedFile;

		cb();
	}, function (cb) {
		// no need to write a manifest file if there's nothing to manifest
		if (Object.keys(manifest).length === 0) {
			cb();
			return;
		}

		getManifestFile(opts, function (err, manifestFile) {
			if (err) {
				cb(err);
				return;
			}

			if (opts.merge && !manifestFile.isNull()) {
				var oldManifest = {};

				try {
					oldManifest = opts.transformer.parse(manifestFile.contents.toString());
				} catch (err) {}

				manifest = objectAssign(oldManifest, manifest);
			}

			manifestFile.contents = new Buffer(opts.transformer.stringify(sortKeys(manifest), null, '  '));
			this.push(manifestFile);
			cb();
		}.bind(this));
	});
}
```
- example usage
```shell
...
'''


## API

### rev()

### rev.manifest([path], [options])

#### path

Type: 'string'
Default: '"rev-manifest.json"'

Manifest file path.
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
