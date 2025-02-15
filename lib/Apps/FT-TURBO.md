


# [How to Use Hotwire Rails: Getting Started Tutorial](https://www.bacancytechnology.com/blog/hotwire-rails-tutorial)

```
mkdir ./railshotwire
cd ./railshotwire
echo "source 'https://rubygems.org'" > Gemfile
echo "gem 'rails', '~> 7.0.0'" >> Gemfile
bundle install  
bundle exec rails new . --force -d=postgresql

echo "class HomeController < ApplicationController" > app/controllers/home_controller.rb
echo "end" >> app/controllers/home_controller.rb

echo "class OtherController < ApplicationController" > app/controllers/other_controller.rb
echo "end" >> app/controllers/other_controller.rb

echo "Rails.application.routes.draw do" > config/routes.rb
echo '  get "home/index"' >> config/routes.rb
echo '  get "other/index"' >> config/routes.rb
echo '  root to: "home#index"' >> config/routes.rb
echo 'end' >> config/routes.rb

mkdir app/views/home
echo '<h1>This is Rails Hotwire homepage</h1>' > app/views/home/index.html.erb
echo '<div><%= link_to "Enter to other page", other_index_path %></div>' >> app/views/home/index.html.erb

mkdir app/views/other
echo '<h1>This is Another page</h1>' > app/views/other/index.html.erb
echo '<div><%= link_to "Enter to home page", root_path %></div>' >> app/views/other/index.html.erb

bin/rails db:create
bin/rails db:migrate

```