# Adding TypeScript

This file documents the steps that were taken to add TypeScript support to this react-native application.

## Overview

...Coming Soon

## Details

#### Install typescript and typings (global)

`npm i -g typescript typings`

#### Initialise typescript settings

`typings init`

#### Modify .gitignore to exclude TS build artifacts

Add the following to your .gitignore, as typescript definitions will be stored in typings, and we will configure to output to the artifacts folder):
```
artifacts
typings
```

#### Install react-native typings

```
typings install dt~react-native --save
typings install dt~react-native-dom --save
```

#### Add a tsconfig.json file (in root folder)

```
{
    "compilerOptions": {
        "module": "commonjs",
        "target": "es6",
        "jsx": "react",
        "outDir": "artifacts",
        "sourceMap": false
    },
    "filesGlob": [
        "typings/index.d.ts",
        "src/**/*.ts",
        "src/**/*.tsx"
    ],
    "exclude": [
        "node_modules",
        "artifacts"
    ]
}
```

#### [Optional] Set VsCode Workspace settings

Modify the .vscode/settings.json file to specify to use the npm installed typescript version. NB: It looks like VsCode doesn't create this folder automatically, so you may need to create the folder and file.

```
{
    "typescript.tsdk": "node_modules/typescript/lib"
}
```

#### [Optional] Add VsCode build settings

Create/Modify the .vscode/tasks.json file to set these

```
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "0.1.0",
    "windows": {
        "command": ".\\node_modules\\.bin\\tsc"
    },
    "linux": {
        "command": "./node_modules/.bin/tsc"
    },
    "osx": {
        "command": "./node_modules/.bin/tsc"
    },
    "isShellCommand": true,
    "args": ["-p", "."],
    "showOutput": "silent",
    "problemMatcher": "$tsc"
}
```

#### Set-up TSLint

```
npm install tslint --save
tslint --init
```

NB: This creates a tslint.json configuration file, and you can 

#### Create src directory for typescript sources:

`mkdir src`

#### Rename and move the starting point scripts

```Windows
move index.android.js src\root.android.tsx
move index.ios.js src\root.ios.tsx
move index.web.js src\root.web.tsx
```

```Linux/OS X
mv index.android.js src/root.android.tsx
mv index.ios.js src/root.ios.tsx
mv index.web.js src/root.web.tsx
```

#### Add new starting point scripts referencing these

- New index.android.js

`require("./artifacts/root.android.js")`

- New index.ios.js

`require("./artifacts/root.ios.js")`

- New index.web.js

`require("./artifacts/root.web.js")`

#### Tweak imports

```
//import React, { Component } from 'react';
import * as React  from "react";
import { Component } from 'react';
```

#### Check that typescript settings are working

`tsc -p .`

This should attempt to build the project and output various javascript files to /artifacts/

#### Test running again

`npm run dev`

#### Test modifying

## Helpful Guides

- <http://www.justin-credible.net/2016/08/23/using-typescript-with-react-native/>
- <https://raygun.com/blog/2016/07/react-native-typescript/>
