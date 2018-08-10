# webpack-4-tutor

Materials:
> https://www.valentinog.com/blog/webpack-tutorial/

> https://www.valentinog.com/blog/from-gulp-to-webpack-4-tutorial/

## Initializing and setting up webpack

Run in a terminal:
```
npm init
npm install --save-dev webpack
npm i webpack-cli --save-dev
npm i html-webpack-plugin html-loader --save-dev
```

Add to package.json build script:
```
"scripts": {
  "build": "webpack"
}
```

Create files with appropriate content:
```
create: webpack.config.js
create: ./scr/index.html
create: ./scr/index.js
```

Content for webpack.config.js:
```
const HtmlWebPackPlugin = require("html-webpack-plugin");
module.exports = {
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [{ loader: "html-loader", options: { minimize: true } }]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "src/index.html",
      filename: "./index.html"
    })
  ]
};
```

Content for ./scr/index.html:
```
<!DOCTYPE html>
<html lang="it">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Webpack</title>
</head>
<body>
  <section class="main">
    <h1>Hello Webpack!!</h1>
  </section>
</body>
</html>

```

Run in a terminal:
```
npm run build
```

Open up package.json and fill the script section like the following:
```
"scripts": {
  "dev": "webpack --mode development",
  "build": "webpack --mode production"
}
```

Run in a terminal:
```
npm run dev
```

Overriding the default output:
```
"scripts": {
  "dev": "webpack --mode development ./foo/src/js/index.js --output ./foo/main.js",
  "build": "webpack --mode production ./foo/src/js/index.js --output ./foo/main.js"
}
```

## Transpiling Javascript ES6 with Babel

Run in a terminal:
```
npm i babel-core babel-loader babel-preset-env --save-dev
```

Create file '.babelrc':
```
{
    "presets": [
        "env"
    ]
}
```

Configuring webpack.config.js for babel loader:
```
module.exports = {
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
};
```

To use babel-loader without a configuration file configure your npm scripts in package.json like so:
```
"scripts": {
    "dev": "webpack --mode development --module-bind js=babel-loader",
    "build": "webpack --mode production --module-bind js=babel-loader"
  }
```

## Setting up webpack with React

Install React with:
```
npm i react react-dom --save-dev
```

Add babel-preset-react:
```
npm i babel-preset-react --save-dev
```

Configure the preset in .babelrc:
```
{
  "presets": ["env", "react"]
}
```

## Configuring babel-loader to read .jsx if necessary:

Open up webpack.config.jsand configure the loader like so:
```
module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }
    ]
  }
};
```

To test things out you can create a dummy React component in ./src/App.js:
```
import React from "react";
import ReactDOM from "react-dom";
const App = () => {
  return (
    <div>
      <p>React here!</p>
    </div>
  );
};
export default App;
ReactDOM.render(<App />, document.getElementById("app"));
```

Next up import the component in ./src/index.js:
```
import App from "./App";
```

## The HTML webpack plugin

Add the dependencies with:
```
npm i html-webpack-plugin html-loader --save-dev
```

Then update the webpack configuration:
```
const HtmlWebPackPlugin = require("html-webpack-plugin");
module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }, {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    })
  ]
};

```

Next up create an HTML file into ./src/index.html:
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>webpack 4 quickstart</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

#### Open up ./dist/index.htmlin your browser: you should see the React component working!

## Extracting CSS to a file

Install the plugin and css-loader with:
```
npm i mini-css-extract-plugin css-loader --save-dev
```

Next up create ./src/main.css for testing things out:
```
body {
    line-height: 2;
}
```

Configure both the plugin and the loader:
```
const HtmlWebPackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }, {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }, {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      }
    ]
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    }),
    new MiniCssExtractPlugin({
      filename: "[name].css",
      chunkFilename: "[id].css"
    })
  ]
};
```

Import the CSS in the entry point ./src/index.js:
```
import style from "./main.css";
```

Enter in a terminal:
```
npm run build
```

## The webpack dev server

To set up webpack dev server install the package with:
```
npm i webpack-dev-server --save-dev
```

Next up open package.jsonand adjust the scripts like the following:
```
"scripts": {
  "start": "webpack-dev-server --mode development --open",
  "build": "webpack --mode production"
}
```

Now, by running next you’ll see webpack dev server launching your application inside the browser:
```
npm run start
```

## To avoid "cannot GET /URL" error in a webpack side configure your webpack like so

```
publicPath: '/',
historyApiFallback: true
```

> https://tylermcginnis.com/react-router-cannot-get-url-refresh/

Example:
```
const HtmlWebPackPlugin = require("html-webpack-plugin")
const MiniCssExtractPlugin = require("mini-css-extract-plugin")
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
    publicPath: '/'
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      }, {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true }
          }
        ]
      }, {
        test: /\.css$/,
        use: [MiniCssExtractPlugin.loader, "css-loader"]
      }
    ]
  },
  devServer: {
    historyApiFallback: true,
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./index.html"
    }),
    new MiniCssExtractPlugin({
      filename: "[name].css",
      chunkFilename: "[id].css"
    })
  ]
}
```

## To add ability to write html attrs in react app

> https://github.com/insin/babel-plugin-react-html-attrs

```
npm install --save-dev babel-plugin-react-html-attrs
```

Then edit your .babelrc to include react-html-attrs:
```
{
  "plugins": [
    "react-html-attrs"
  ]
}
```

## To add the rest of es6 features

```
npm install --save webpack-preset webpack-preset-babel webpack-preset-babel-stage-1
```

Config your webpack like so:
```
module: {
  rules: [
    {
      use: {
        loader: "babel-loader",
        query: {
          presets: ['react', 'es2015', 'stage-1']
        }
      }
    }
  ]
}
```
