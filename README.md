# react-tutor
Materials:
> https://www.valentinog.com/blog/webpack-tutorial/

> https://www.valentinog.com/blog/from-gulp-to-webpack-4-tutorial/

> https://www.youtube.com/watch?v=MhkGQAoc7bc&t=73s&list=PLoYCgNOIyGABj2GQSlDRjgvXtqfDxKm5b&index=2

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

Create files:
```
create: webpack.config.js
create: ./scr/index.html
create: ./scr/index.js
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
