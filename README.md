# Creating project

## Installing

```sh
git init

npm init -y

npm i -D webpack webpack-cli webpack-dev-server mini-css-extract-plugin html-webpack-plugin clean-webpack-plugin style-loader css-loader node-sass sass-loader ts-loader typescript babel-core babel-loader babel-preset-es2015 @babel/core @babel/preset-env

tsc --init
```

## Configure

### webpack.config.js

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: "development",
  entry: "./src/typescript/index.ts",
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist")
  },
  devServer: {
    contentBase: "./dist"
  },
  devtool: "inline-source-map",
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: ["babel-loader", "ts-loader"],
        exclude: /node_modules/
      },
      {
        test: /\.s[ac]ss$/i,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            options: {
              hmr: process.env.NODE_ENV === "development",
              publicPath: "./dist/styles/"
            }
          },
          "css-loader",
          {
            loader: "sass-loader",
            options: {
              sourceMap: true
            }
          }
        ]
      }
    ]
  },
  resolve: {
    extensions: [".tsx", ".ts", ".js"]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html"
    }),
    new MiniCssExtractPlugin({
      filename: "styles.css"
    }),
    new CleanWebpackPlugin()
  ]
};
```

### package.json scripts

```json
"scripts": {
    "start": "webpack-dev-server --open",
    "watch": "webpack --watch",
    "build": "webpack"
  },
```

### tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "allowJs": true,
    "sourceMap": true,
    "outDir": "./dist",
    "strict": true,
    "noImplicitAny": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true
  }
}
```

### .babelrc

```json
{
  "presets": ["@babel/preset-env"]
}
```

### .gitignore

```
/dist
/node_modules
```

## File structure

```
my-project
|- /src
  |- /styles
    |- style.scss
  |- /typescript
    |- index.ts
  |- index.html
|- .babelrc
|- .gitignore
|- package.json
|- tsconfig.json
|- webpack.config.js
```
