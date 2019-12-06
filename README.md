# README

- Created Git Repo
- Cloned down Git Repo to local repo
- rails new ruby_user_auth -G -d postgresql // initialized new ruby project with postgresql as database
- created .gitignore in root project directory
- added the following in the gitignore file
    node_modules/
    bundle.js
    bundle.js.map
- update gemfile to include:
    better_errors
    binding_of_caller
    pry-rails
    annotate
    bcrypt
    jquery-rails (When using rails 5.1+)
- in 'application.js' add the following:
    //= require jquery
    //= require jquery_ujs
-  bundle install
- run 'npm init --yes' in root project directory
- make 'frontend' folder in root project directory'
- within the frontend folder, create your entry file.  can be 'index.jsx' or whatever you want
- in root, run
    npm install --save webpack webpack-cli react react-dom react-router-dom redux react-redux @babel/core @babel/preset-react @babel/preset-env babel-loader
- create webpack.config.js file
- add the following in the config replacing the entry file name with the actual file name that you created

``` javascript
const path = require("path");

module.exports = {
  context: __dirname,
  entry: "./frontend/trackmania.jsx",
  output: {
    path: path.resolve(__dirname, "app", "assets", "javascripts"),
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /(node_modules)/,
        use: {
          loader: "babel-loader",
          query: {
            presets: ["@babel/env", "@babel/react"]
          }
        }
      }
    ]
  },

  devtool: "source-map",
  resolve: {
    extensions: [".js", ".jsx", "*"]
  }
};
```

-