# resolve-package-path [![Build Status](https://travis-ci.org/stefanpenner/fast-resolve-package.svg?branch=master)](https://travis-ci.org/stefanpenner/fast-resolve-package) [![Build status](https://ci.appveyor.com/api/projects/status/7d7xx9ig4153lhh1/branch/master?svg=true)](https://ci.appveyor.com/project/embercli/resolve-package-path/branch/master)


This project is special-purpose, made to resolve a package.json file
given a specific module name and basedir to start searching from. It
cannot and does not resolve anything else.

To achieve its file-resolution performance, it does two specific things:

* It memoizes results identically to node's `require`. Specifically,
  for a given moduleName and baseDir it will, for the duration of the process,
  always return the exact same response.

* It re-implements the parts of `require.resolve` needed to resolve package.json
  files ONLY. This removes unneeded I/O. (based on @davecombs approach)

## Usage

```sh
yarn add resolve-package-path
```

```js
const resolvePackage = require('resolve-package-path');

resolvePackage('rsvp', 'base-dir/to/start/the/node_resolution-algorithm-from') => // /path/to/rsvp.json or null
```


## Advanced usage


### Disable default caching

Although by default `resolve-package-path` caches or memoizes results, this feature can be disabled:

```js
const resolvePackage = require('resolve-package-path');

resolvePackage('rsvp', 'base-dir/to/start/the/node_resolution-algorithm-from', false) => // uncached result /path/to/rsvp.json or null
```

### Purge the cache

```js
const resolvePackage = require('resolve-package-path');
resolvePackage._resetCache();
```

### Providing an alternative cache

In some advanced circumtances, you may want to gain access to the cache to share between more systems.
In that case, a cache instance of the following form can be provided as a third argument:

```
cache = {
  RESOLVED_PACKAGE_PATH: new Map(),
  REAL_FILE_PATH: new Map(),
  REAL_DIRECTORY_PATH: new Map()
};

const resolvePackage = require('resolve-package-path');
resolvePackage('rsvp', 'path/to/start/from', cache);
```
