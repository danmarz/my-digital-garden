---
Title: Designing an API from scratch with NodeJS
---

## Directory setup
- `mkdir movies-api && cd "$_"`
```shell
|__\dist
	|____\css
	.	  	|styles.cc
	|____\img
	.	  	|\svg
	|____\js
	.	  	|bundle.js
	|____index.html
|__\src
	|____\js
	.    	|index.js
	.	 	|module.js
	|____index.html
```
- `npm init`
- `npm install webpack --save-dev` <- Install Webpack as dev dependency
- `npm install webpack-cli --save-dev`<- specify command-line args to webpack (..in scripts)
- `npm install webpack-dev-server --save-dev`<- live refresh server
- `npm install html-webpack-plugin --save-dev` <- allows templates & injection of scripts in html
- `npm install --save-dev @babel/core @babel/preset-env babel-loader` <- Babel transpiler
- `(sudo) npm install live-server --global` <- local Go Live env
- `echo "node_modules/" > .gitignore`
- `npm install --save @babel/polyfill` -> _deprecated_
- `touch ./webpack.config.js` <- create the webpack config file

## Webpack config
```js
const path = require('path'); // 'path' module required for output file
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {			// 4 main concepts: entry, output, loaders, plugins
	
	entry: './src/js/index.js',  // entry point (relative route to config file)
	output: {
		path: path.resolve(__dirname, 'dist'), // {dir}/dist/ +
		filename: 'js/bundle.js'			   // /js/bundle.js
	},
	
	// mode:'development' or 'production'-> extracted to 'script' in package.json
	
	devServer: {
		static:'./dist'		// from where to serve static html/css/img etc
	},
	plugins: [
		new HtmlWebpackPlugin({ 	// copy & use template /src/index.html into /dist/*
			filename: 'index.html',
			template: './src/index.html'
		})
 	],
	 module: {
 		rules: [
 			{
 				test:/\.js$/, // match all .js files
 				exclude:/node_modules/, // exclude /node_modules
 				use: {
 					loader:'babel-loader' // load babel (.babelrc)
 				}
 			}
 		]
 	} 
};
```

## Package.json config
```js
{
 "name": "movies-api",
 "version": "1.0.0",
 "description": "",
 "main": "index.js",
 "scripts": {
 	"dev": "webpack --mode development",	// npm run dev -> goto webpack config entry
 	"build": "webpack --mode production",	// nepm run build
 	"init": "webpack-dev-server --mode development --open"	// live init/start &-open browser
 },
 "author": "dan m.",
 "license": "MIT",
 "devDependencies": {
	"webpack": "^5.57.1",
 	"webpack-cli": "^4.8.0",
 	"webpack-dev-server": "^4.3.1"
 }
}
```

## .babelrc config
```js
{
 "presets":[
	 [
 		"@babel/env", {
 			"targets":{
 				"browsers":[
 					"last 5 versions",
 					"ie >= 8"
 					]
 				}
 			}
 		]
 	]
}
```

# [[javascript]] named and default imports and exports
```js
import output from './models/Search';	// for export default _func/class/literal_
import { renderDetails as rd, 
		 renderHowOld as rh,
		 age as a
		} from './views/searchView';	// import multiple _func/class/literal_
import * as searchView from './views/searchViews';	// import _all_ -> use searchView.[dot]xx
```

## JS async fetch function
```js
async function testApi(query){
	const key = `ac8e4d79`;
	
	try {
		const result = await fetch(`http://www.omdbapi.com/?apikey=${key}&s=${query}`);
		const data = await result.json();
		
		console.log(data.Search)
	} catch (err) {
		console.log(err);
	}
}
```

## Use labels for long functions

1. Define, long convoluted function names in a classSelector.js file within an _includes_ folder:
```js
export const classNames = {

 formInput:document.querySelector('.top-search__input'),
}
```

2. Then import the file in the _views_, _models_ or _controllers_ where needed:
```js
import { classNames } from "../includes/classSelector";
export const getFormInput = () => classNames.formInput.value;
```
3. ??? profit!


