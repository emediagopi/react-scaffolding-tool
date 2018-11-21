https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658

https://github.com/paradoxinversion/creating-a-react-app-from-scratch
https://github.com/paradoxinversion/react-starter


Setup
1.	npm init
2.	git init
3.	create folder public and add index.html with below lines
<!-- sourced from https://raw.githubusercontent.com/reactjs/reactjs.org/master/static/html/single-file-example.html -->
<!DOCTYPE html>
<html>

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  <title>React Starter</title>
</head>

<body>
  <div id="root"></div>
  <noscript>
    You need to enable JavaScript to run this app.
  </noscript>
  <script src="../dist/bundle.js"></script>
</body>

</html>

Babel
npm install --save-dev @babel/core@latest	Main package. To do any transformations
npm install --save @babel/cli@latest	Compile files from the command line
npm install --save @babel/preset-env@latest	Preset allows us to transform ES6+ into more traditional javascript
npm isntall --save @babel/preset-react@next 	Does the same as env do with JSX instead.
Create a file called 
".babelrc" with following line	telling babel that we are going to use the "env" and "react" presets
//.babelrc
{
  "presets": ["@babel/env", "@babel/preset-react"]
}

WebPack
npm install --save-dev webpack	Main package. To do any transformations
npm install --save-dev webpack-cli	Compile files from the command line
npm install --save webpack-dev-server	Preset allows us to transform ES6+ into more traditional javascript
npm install --save style-loader@latest	Does the same as env do with JSX instead.
npm install --save css-loader@latest	telling babel that we are going to use the "env" and "react" presets
npm install --save babel-loader@latest	
create a new file in root folder called  webpack.config.js	•	entry: - where out application starts and where to start building our files
•	module: - How your exported javascript modules are transformed and which ones are included according to the given array of rules.
o	First rule is transforming ES6 and JSX
o	test and exclude:- conditions to match file against. in this case, it will match anything outside of the "node_modules" and "bower_components" directories
•	env:- preset in options

Processing CSS
•	use:- to add style-loader and css-loader
o	loader is the shorthand for the "use" property, when only one loader is being utilized


•	resolve: - allows us to specify which extensions webpack will resolve -- this allows us to import modules without needing to add their extensions.
•	output:- where to pull out our bundled code
o	publicpath:- what directory the bundle should go in, and also tell "webpack-dev-server" where to serve files from.
o	publicpath, it specifies the public URL of the directory. If this is not set correctly, you will get 404 error
•	Hot Module Replacement:- We don't want to constantly refresh to see our changes

//webpack.config.js
const path = require("path");
const webpack = require("webpack");

module.exports = {
  entry: "./src/index.js",
  mode: "development",
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /(node_modules|bower_components)/,
        loader: "babel-loader",
        options: { presets: ["@babel/env"] }
      },
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"]
      }
    ]
  },
  resolve: { extensions: ["*", ".js", ".jsx"] },
  output: {
    path: path.resolve(__dirname, "dist/"),
    publicPath: "/dist/",
    filename: "bundle.js"
  },
  devServer: {
    contentBase: path.join(__dirname, "public/"),
    port: 3000,
    publicPath: "http://localhost:3000/dist/",
    hotOnly: true
  },
  plugins: [new webpack.HotModuleReplacementPlugin()]
};



React
npm install --save react@latest	
npm install --save react-dom@latest	

//index.js
import React from "react";
import ReactDOM from "react-dom";
import App from "./App.js";
ReactDOM.render(<App />, document.getElementById("root")) //ReactDOM.render:- What to render and where to render

//App.js
import React, { Component} from "react";
import "./App.css";

class App extends Component{
  render(){
    return(
      <div className="App">
        <h1> Hello, World! </h1>
      </div>
    );
  }
}

export default App;

//App.css
.App {
  margin: 1rem;
  font-family: Arial, Helvetica, sans-serif;
}

package.json
"scripts":{
    "start":"webpack-dev-server --mode development"
}

Finishing HMR
If you run the server now, you'll notice none of your changes actually cause anything to happen in the client.

HMR need to know what to actually replace and currently we haven't given it anything. for that, we are going to use a package one of the folks on the react team have provided us: "react-hot-loader" 

npm install --save react-hot-loader@latest


Add the react-hot-loader in App.js
import React, { Component} from "react";
import {hot} from "react-hot-loader";
import "./App.css";

class App extends Component{
  render(){
    return(
      <div className="App">
        <h1> Hello, World! </h1>
      </div>
    );
  }
}

export default hot(module)(App);


Build Details

Built files never shows up in you dist directory. The wepack-dev-server is actually serving the bundled files from memory (once the server stops, they are gone). To actually build your files, we are going to utilize webpack proper (add a script called build in your package.json with the following command: webpack --mode development ).

you can replace development with production , but if you completely --omit , it will fall back to the latter and give you a warning

The final package.json File will be
{
  "name": "scaffoldingTool",
  "version": "1.0.0",
  "description": "setup made from scratch",
  "main": "index.js",
  "author": "gopinath.r",
  "license": "MIT",
  "scripts": {
    "start": "webpack-dev-server --mode development"
  },
  "devDependencies": {
    "@babel/core": "^7.1.6",
    "webpack": "^4.26.0",
    "webpack-cli": "^3.1.2"
  },
  "dependencies": {
    "@babel/cli": "^7.1.5",
    "@babel/preset-env": "^7.1.6",
    "@babel/preset-react": "^7.0.0",
    "babel-loader": "^8.0.4",
    "css-loader": "^1.0.1",
    "react": "^16.6.3",
    "react-dom": "^16.6.3",
    "react-hot-loader": "^4.3.12",
    "style-loader": "^0.23.1",
    "webpack-dev-server": "^3.1.10"
  }
}


Evernote helps you remember everything and get organized effortlessly. Download Evernote. 


