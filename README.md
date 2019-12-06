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

- run 'rails g model user username:string password_digest:string session_token:string' to create model and migration
- add necessary validations in the migration file -app -> db -> migrate
- open postgres
- run "rails db:create'
- run 'rails db:migrate' to create migration and tables within database
- in model/user.rb add the following:
``` ruby
    class User < ApplicationRecord
  validates :password, length: { minimum: 6 }, allow_nil: true
  validates :username, :password_digest, presence: true, uniqueness: true
 
  before_validation :ensure_token

  attr_reader :password
  has_many :projects
  
  has_many :stories,
  foreign_key: :requestor_id,
  class_name: :Story


  def password=(password)
    self.password_digest = BCrypt::Password.create(password)
    @password = password
  end

  def is_password?(password)
    BCrypt::Password.new(self.password_digest).is_password?(password)
  end

  def self.find_by_creds(username, password)
    @user = User.find_by(username: username)
    @user && @user.is_password?(password) ? @user : nil
  end

  def ensure_token
     self.session_token ||= SecureRandom.urlsafe_base64
  end 

  def reset_token
    self.session_token = SecureRandom.urlsafe_base64
  end

end

```

- run 'rails g controller api/users' to create controller "Users" under the api folder
- add the following to the routes.rb file

``` ruby
     root to: 'root#root'

    namespace :api, defaults: { format: :json } do
        resources :users, only: [ :index, :create ]
        resource :session, only: [ :create, :destroy ]
    end
```

- in the users_controller.rb file add:
``` ruby
    class Api::UsersController < ApplicationController

  def create
    @user = User.new(user_params)
    if @user.save
      login(@user)
      render :show
    else
      render json: @user.errors.full_messages, status: 401
    end
  end

  def user_params
    params.require(:user).permit(:username, :password)
  end
end

```
