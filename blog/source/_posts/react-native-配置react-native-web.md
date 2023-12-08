---
title: react native 配置react-native-web
date: 2022-12-09 10:31:10
tags: react-native
categories: 技术
thumbnail: https://cdn.jsdelivr.net/gh/LiuIos/picBed@master/002.jpg
---

# react native 配置react-native-web

**1.安装依赖**

```bash
yarn add react-dom react-native-web
```

webpack相关和 `babel-plugin-react-native-web`

```bash
yarn add babel-plugin-react-native-web babel-loader url-loader webpack webpack-cli webpack-dev-server html-webpack-plugin --dev
```

**2.根目录新建 `web/webpack.config.js`**

```js
// web/webpack.config.js

const path = require('path');
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');

const appDirectory = path.resolve(__dirname, '../');
const {presets, plugins} = require(`${appDirectory}/babel.config.js`);

const compileNodeModules = [
  // 这个位置，要加上所有的你用到的RN的库，因为需要把这些RN的库，都通过RNW编译成Web的代码
 
  '@react-navigation/bottom-tabs',
  '@react-navigation/native',
  '@react-navigation/native-stack',
  '@react-navigation/elements',
  '@react-navigation/stack',
  // 'react-native-linear-gradient',
  'react-native-safe-area-context',
  'react-native-screens',
  'react-native-root-toast',
  'react-native-root-siblings',
  'prop-types',
  'react-native-storage',
  'react-native-svg',
].map(moduleName => path.resolve(appDirectory, `node_modules/${moduleName}`));

// This is needed for webpack to compile JavaScript.
// Many OSS React Native packages are not compiled to ES5 before being
// published. If you depend on uncompiled packages they may cause webpack build
// errors. To fix this webpack can be configured to compile to the necessary
// `node_module`.
const babelLoaderConfiguration = {
  test: /\.(js|jsx|ts|tsx)$/,
  // Add every directory that needs to be compiled by Babel during the build.
  include: [
    path.resolve(appDirectory, 'index.web.js'),
    path.resolve(appDirectory, 'App.js'),
    path.resolve(appDirectory, 'src'),
    path.resolve(appDirectory, 'component'),
    path.resolve(appDirectory, 'web'),
    path.resolve(appDirectory, 'node_modules/react-native-uncompiled'),
    ...compileNodeModules,
  ],
  use: {
    loader: 'babel-loader',
    options: {
      cacheDirectory: true,
      // The 'metro-react-native-babel-preset' preset is recommended to match React Native's packager
      //   presets: ['module:metro-react-native-babel-preset'],
      // Re-write paths to import only the modules needed by the app
      plugins: [
        'react-native-web',
        '@babel/plugin-proposal-export-namespace-from',
        ['@babel/plugin-proposal-decorators', {legacy: true}],
        'react-native-reanimated/plugin',
        [
          '@babel/plugin-transform-modules-commonjs',
          {
            allowTopLevelThis: true,
          },
        ],
      ],
      presets,
      // plugins,
      // env: {
      //   production: {
      //     plugins: ['transform-remove-console'],
      //   },
      // },
    },
  },
};

const svgLoaderConfiguration = {
  test: /\.svg$/,
  use: [
    {
      loader: '@svgr/webpack',
    },
  ],
};

// This is needed for webpack to import static images in JavaScript files.
const imageLoaderConfiguration = {
  test: /\.(gif|jpe?g|png|svg)$/,
  use: {
    loader: 'url-loader',
    options: {
      name: '[name].[ext]',
      esModule: false,
    },
  },
};

module.exports = {
  entry: [
    'babel-polyfill',
    // load any web API polyfills
    // path.resolve(appDirectory, 'polyfills-web.js'),
    // your web-specific entry file
    path.resolve(appDirectory, 'index.web.js'),
  ],

  // configures where the build ends up
  output: {
    filename: 'bundle.web.js',
    path: path.resolve(appDirectory, 'dist'),
  },

  // ...the rest of your config

  module: {
    rules: [
      babelLoaderConfiguration,
      imageLoaderConfiguration,
      svgLoaderConfiguration,
      // {
      //   test: /\.(tsx|ts)$/,
      //   exclude: /node_modules/,
      //   loader: 'ts-loader',
      // },
    ],
  },

  resolve: {
    // This will only alias the exact import "react-native"
    alias: {
      'react-native$': 'react-native-web',
    },
    // If you're working on a multi-platform React Native app, web-specific
    // module implementations should be written in files using the extension
    // `.web.js`.
    extensions: [
      '.web.js',
      '.web.jsx',
      '.js',
      '.jsx',
      '.tsx',
      '.ts',
      '.web.tsx',
      '.web.ts',
      'component.js',
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, './index.html'), // 指定模板路径
      filename: 'index.html', // 指定文件名
    }),
    new webpack.HotModuleReplacementPlugin(),
    new webpack.DefinePlugin({
      // 此处特别说明，是为了兼容RN和RNW之间的一个全局变量而特别加上的
      // 具体可以看RNW的作者Necolas的说明
      // <https://github.com/necolas/react-native-web/issues/349>
      __DEV__: JSON.stringify(true),
      process: {env: {}},
    }),
    new webpack.EnvironmentPlugin({JEST_WORKER_ID: null}),
  ],
};

```

**3.根目录新建  `web/index.html` 文件**

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1, shrink-to-fit=no, user-scalable=no"
    />
    <!-- <meta name="theme-color" content="#000000" /> -->
    <title>安全监测</title>
    <script src="https://cdn.bootcss.com/jsencrypt/3.0.0-beta.1/jsencrypt.js"> </script>
    <style>
      @media only screen and (max-width: 700px) {
        html,
        body {
          /* 14/(375/100) */
          font-size: 3.73333333333vw;
        }
      }

      @media only screen and (min-width: 700px) {
        html,
        body {
          font-size: 20px;
        }
        /* #app-root {
          max-width: 700px;
        } */
      }
        body {
            justify-content: center;
            /* align-items: center; */
            display: flex;
            height: 100vh;
            overflow: hidden;
        }
      #app-root {
        display: flex;
        flex: 1 1 100%;
        /* height: 100vh; */
        height:100%;
        max-width: 700px;
        align-self: center;
        flex-direction: column;
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <div id="app-root"></div>
  </body>
</html>
```

**4.根目录新建 ``index.web.js`` 文件**

```js
//index.web.js

import {AppRegistry} from 'react-native';
import {name as appName} from './app.json';
import App from './App.js';

AppRegistry.registerComponent(appName, () => App);
AppRegistry.runApplication(appName, {
  initialProps: {},
  rootTag: document.getElementById('app-root'),
});
```

**5.配置 package.json 启动命令**

```
"web": "webpack serve --mode=development --config ./web/webpack.config.js",
"build-web": "webpack --mode=production --config ./web/webpack.config.js"
```

**6.现在可以启动项目了**

```
yarn web
```

