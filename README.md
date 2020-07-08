# webpack
Simple usage of Webpack 

## Init
Init empty package.json with default params (by using `-y`)

`npm init -y`

Note: name in package.json will not be 'webpack' 


- Now we need to instal `webpack` as dev dependency
`npm install webpack --save-dev`

- add  `npm run build` and `npm run start` scripts to package.json
```
"scripts": {
  "build": "webpack",
  "start": "webpack --watch"
},
```

- create index.html in root src folder, src/app.js
In `index.html` add link to `bundle.js`. In this moment we don't have this file, webpack will create it  from app.js 

```<script type="text/javascript" src="dist/bundle.js"></script>```

- Create webpack configuration file `webpack.config.js`
```
const path = require('path');

module.exports = {
  entry: './src/app.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  }
};
```
`Entry` - our entry point file is src/app.js
`Output` - our output file will be called as 'dist/bundle.js'

- create `js` folder inside `src` and  `src/js/1.js` and `src/js/2.js` files

`src/js/1.js`
```
console.log('This is 1.js');
```

`src/js/2.js`
```
console.log('This is 2.js');
```

- Now we need to import our js files to `app.js`
```
import './js/1.js';
import './js/2.js';
```

- Now we need to run webpack by using `npm run build` command. Webpack will create `dist` folder with `bundle.js` file. Now we will see `console.log` messages in browser devtools

### Add SCSS
- create `scss` folder in `src`
- create `main.scss` file
- add css code to file 

```
$bg-color: pink;

body {
 background: $bg-color;
}

```

- To import our new `main.scss` file add this import to `src/app.js`
```
import './scss/main.scss'
```

#### Add css loaders
```
npm install style-loader css-loader sass-loader node-sass extract-text-webpack-plugin -D

```

- Add  `Extract Text Plugin` plugin to `webpack.config.js` 
```
const ExtractTextPlugin = require('extract-text-webpack-plugin')
```

- Now need to tell webpack to handle scss files
```
output: {
  ...
},
module: {
  rules: [
    {
      test: /\.scss$/,
      use: ExtractTextPlugin.extract({
        fallback: 'style-loader',
        use: ['css-loader', 'sass-loader']
      })
    }
  ]
}
```

- Create reference to `Extract Text Plugin`. It will tell webpack that css files need to merge to one file and call it as `style.css` 
```
plugins: [
 new ExtractTextPlugin('style.css')
]
```

- Now `webpack.config.js` looks like this
```
const path = require('path');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
  entry: './src/app.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'none',
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: ExtractTextPlugin.extract({
          fallback: 'style-loader',
          use: ['css-loader', 'sass-loader']
        })
      }
    ]
  },
  plugins: [
    new ExtractTextPlugin('style.css')
  ]
};

```

### Add link reference to our css file
```
<link rel='stylesheet' href='dist/main.css'>
```

### Run build
```
npm run build
```

#### Note: If you get Deprecation warning you can use `mini-css-extract-plugin` plugin instead of `ExtractTextPlugin`
```
npm install --save-dev mini-css-extract-plugin
```


```
const path = require('path');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry: './src/app.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  mode: 'none',
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [MiniCssExtractPlugin.loader, 'css-loader', 'sass-loader'],
      }
    ]
  },
  plugins: [new MiniCssExtractPlugin()],
};
```


### Add another scss file `custom.scss` and add to `src/app.js`
``` import './scss/custom.scss' ```

- Run `npm run start` to watch changes