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