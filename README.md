# rollup-plugin-import-resolver

Resolves import statements by using aliases and file extensions when bundling with rollup.
Also resolves the file when importing a directory.

## Install

```bash
yarn add --dev rollup-plugin-import-resolver

# OR

npm install --save-dev rollup-plugin-import-resolver
```

## Options

```js
import importResolver from "rollup-plugin-import-resolver";

importResolver({
    // a list of file extensions to check, default = ['.js']
    extensions: ['.js', '.vue'],
    // a list of aliases, default = {}
    alias: {
        'lib': './node_modules/otherlib/src'
    },
    // index file name without extension, default = 'index'
    indexFile: 'index',
    // path to node_modules dir, default = ./node_modules
    modulesDir: '/path/to/node_modules'
});

// if called without options, the defaults are
defaultOptions = {
    extensions: ['js'],
    alias: {},
    indexFile: 'index',
    modulesDir: './node_modules'
};
```

## Usage scenarios

Consider the following project structure

    |-- .. 
    '-- src
       |-- ..
       |-- index.js
       '-- utils
           |-- ..
           |-- util1.js
           |-- util2.jsm
           '-- index.js


and plugin options

```json
{
    "extensions": [".js", ".jsm"],
    "alias": {
        "somelib": "./node_modules/other_lib/src/"
    }
}
```

### Resolve "index" file if import points to a directory

```js
// src/index.js

import * as utils from "./utils"; 
// resolved to ./utils/index.js
```

### Resolve extensions

```js
// src/utils/index.js

import util1 from "./util1"; 
// resolved to ./util1.js
import util2 from "./util2"; 
// resolved to ./util2.jsm

export {util1 as u1, util2 as u2};
```

### Resolve aliases

```js
// src/utils/util1.js

import somelib from "somelib";
// resolved to ./node_modules/other_lib/src/index.js

import component1 from "somelib/component1";
// resolved to ./node_modules/other_lib/src/component1.js
// OR if component1 is a folder ./node_modules/other_lib/src/component1/index.js
```

### Resolve modules dir

```js
import something from "vendorlib/file.js";
// resolved to /path/to/node_modules/vendorlib/file.js
```