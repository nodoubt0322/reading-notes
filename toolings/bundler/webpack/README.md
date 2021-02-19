# Install

```js
npm install webpack webpack-cli --save-dev
//webpack 4.x之後, webpack跟webpack-cli就分離開來
```

```js
//package.json

"scripts": {
    "start": "webpack --config production.config.js"
}
//用--config指定用哪一個設定檔
//沒設定的話, 會載入project根目錄下面的 webpack.config.js
```

```js
./node_modules/.bin/webpack
// package安裝完會自動在bin目錄下創建link
// 通過npm script可以直接執行到webpack
```

# Concepts

- Entry: 配置模塊的入口，可簡單理解為輸入
- Output: 配置如何輸出為最終的代碼
- Loaders: 把不同種類的檔案，轉成 webpack 看的懂的 module
- Plugins: 加強 webpack 功能，作用於構建過程
- Mode: 設定開發環境

```js
//webpack.config.js

const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const config = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  plugins: [new HtmlWebpackPlugin({ template: "./src/index.html" })],
  mode: "development",
};
module.exports = config;
```

# entry / output

```js
// 單入口時的配置
module.exports = {
  entry: "./path/to/my/entry/files.js",
  output: {
    filename: "bundle.js",
    path: __dirname + "/dist",
  },
};

// 多入口時的配置
module.exports = {
  entry: {
    home: ".src/app.js",
    about: "./src/adminApp.js",
  },
  output: {
    filename: "[name].js",
    path: __dirname + "/dist",
  },
};
//用站位符[name],會在dist目錄產生home.js跟about.js
```

# loaders

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

const config = {
  entry: "./src/index.js",
  output: {
    path: path.resolve(__dirname, "build"),
    filename: "bundle.js",
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
        // test是匹配規則
        // use指令使用的loader名稱,由右到左, 依序套用
      },
    ],
  },
};
module.exports = config;
```

# plugins

### 常見 plugin

- `CommonsChunkPlugin`: 將 chunks 相同的模塊代碼提取成公共 js
- CleanWebpackPlugin: 清理構建目錄
- `ExtractTextWebpackPlugin`: 將 css 從 bundle 文件裡提取成一個獨立的 css 文件
- CopyWebpackPlugin: 將文件或者文件夾拷貝到構建的輸出目錄
- `HtmlWebpackPlugin`: 創建 html 文件去承載輸出的 bundle
- UglifyjsWebpackPlugin: 壓縮 js
- MiniCssExtractPlugin: 把 css 抽出來成獨立的文件

```js
const config = {
 entry:'./src/index.js',
 output:{...},
 module:{...},
 plugins:[
  new HtmlWebpackPlugin({template:'./src/index.html'})
 ]
}
module.exports = config
```

# mode

### 分三種(webpack4.x 之後才有)

- production
- development
- none

### 設成 development, 會開啟

- NamedChunksPlugin
- NamedModulesPlugin

### 設成 production, 會開啟

- FlagDependencyUsagePlugin
- FlagIncludedChunksPlugin
- ModuleConcatenationPlugin
- NoEmitOnErrorsPlugin
- OccurrenceOrderPlugin
- SideEffectsFlagPlugin
- TerserPlugin
