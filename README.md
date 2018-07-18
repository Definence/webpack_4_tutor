# react-tutor
Materials:
> https://www.valentinog.com/blog/webpack-tutorial/

> https://www.valentinog.com/blog/from-gulp-to-webpack-4-tutorial/

> https://www.youtube.com/watch?v=MhkGQAoc7bc&t=73s&list=PLoYCgNOIyGABj2GQSlDRjgvXtqfDxKm5b&index=2

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
