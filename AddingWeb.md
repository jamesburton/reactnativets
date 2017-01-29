Starting with react-native:-

Following https://raygun.com/blog/2016/07/react-native-typescript/

npm i -g watchman react-native-cli yarn

#### Add web/browser support (See https://github.com/necolas/react-native-web)
npm install --save react@15.4 react-native-web

#### Modify web.config to alias react-native to react-native-web
*** Currently we don't have webpack installed, so skipping and testing

```
module.exports = {
  // ...other configuration
  resolve: {
    alias: {
      'react-native': 'react-native-web'
    }
  }
}
```

#### Install watchman 
- For windows, download the alpha build (see <http://bit.ly/watchmanwinalpha>)
- For Mac `brew install watchman`
- For linux, install from source (see <https://facebook.github.io/watchman/docs/install.html>)

#### Test that the packager runs:
    - NB: Checking for errors, or starting packager, will need to terminate with Ctrl+C
`npm start`

`... check for errors, then exit ...`

`<Ctrl+C>`

#### Add "dev" script to "scripts" in package.json:-

`"dev": "webpack-dev-server --port 3000 --config web/webpack.config.dev.js --inline --hot --colors --quiet",`

#### Install webpack and webpack-dev-server
`npm i --save-dev webpack webpack-dev-server`

#### Create web directory
mkdir web

#### Enter web directory
cd web

#### Add webpack.config.dev.js (in web folder)

```const path = require('path');
const webpack = require('webpack');

module.exports = {
  devServer: {
    contentBase: path.join(__dirname, 'src')
  },
  entry: [
    path.join(__dirname, '../index.web.js')
  ],
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: { cacheDirectory: true }
      },
      {
        test: /\.(gif|jpe?g|png|svg)$/,
        loader: 'url-loader',
        query: { name: '[name].[hash:16].[ext]' }
      }
    ]
  },
  output: {
    filename: 'bundle.js'
  },
  plugins: [
    new webpack.DefinePlugin({
      'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV || 'development')
    }),
    new webpack.optimize.DedupePlugin(),
    new webpack.optimize.OccurenceOrderPlugin()
  ],
  resolve: {
    alias: {
      'react-native': 'react-native-web'
    }
  }
};
```

#### Create src folder (in web folder):
`mkdir src`

#### Add default (react-style) web index page (web/src/index.html), e.g. notepad src\index.html:

```
<!DOCTYPE html>
<meta charset="utf-8">
<title>React Native for Web</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<div id="react-root"></div>
<script src="/bundle.js"></script>
```

#### Install babel-loader (required for webpack to work here)

`npm i --save-dev babel-loader`

#### Return to root folder e.g.

`cd ..\`

#### Add web entry point (<root>/index.web.js):

```
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from 'react';
import {
  AppRegistry,
  StyleSheet,
  Text,
  View
} from 'react-native';

export default class reactnativets extends Component {
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
        <Text style={styles.instructions}>
          To get started, edit index.android.js
        </Text>
        <Text style={styles.instructions}>
          Double tap R on your keyboard to reload,{'\n'}
          Shake or press menu button for dev menu
        </Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

AppRegistry.registerComponent('App', () => reactnativets);
AppRegistry.runApplication('App', {
  rootTag: document.getElementById('react-root')
});
```

#### Run the app in a browser:

`npm run dev`

### Coming Soon

- Add production webpack build and hosting instructions
- Modify example to share core application
