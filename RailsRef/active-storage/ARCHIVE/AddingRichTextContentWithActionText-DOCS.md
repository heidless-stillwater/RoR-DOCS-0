
# [Adding Rich Text Content to Your Rails App With ActionText](https://medium.com/swlh/adding-rich-text-content-to-your-rails-app-with-actiontext-b0a059bcae8f)
  - ### [Action Text Overview](https://guides.rubyonrails.org/action_text_overview.html)
```
rails _7.2.2.1_ new actiontext_demo_1 -j esbuild --css bootstrap --database=postgresql

cd $!

bundle install
rails db:prepare

#######
# MODEL
#
rails generate scaffold Post title:string
rails db:migrate

###########
# routes.rb
#
vi config/routes.rb
--
Rails.application.routes.draw do
  root to: "posts#index"
  resources :posts
end

##########
# TEST
rails s

--

#########################
# ActionText INSTALLATION ????????????????????????
#
rails action_text:install

vi app/javascript/application.js
--
require("trix")
require("@rails/actiontext")
--

vi app/assets/stylesheets/application.scss
--
/*
* This is a manifest file that'll be compiled into application.css, which will include all the files
* listed below.
*
* Any CSS and SCSS file within this directory, lib/assets/stylesheets, or any plugin's
* vendor/assets/stylesheets directory can be referenced here using a relative path.
*
* You're free to add application-wide styles to this file and they'll appear at the bottom of the
* compiled file so the styles you add here take precedence over styles defined in any other CSS/SCSS
* files in this directory. Styles in this file should be added after the last require_* statement.
* It is generally better to create a new file per style scope.*
*= require_tree .
*= require_self
*/
@import "./actiontext.scss";

--

############
# REBUILD DB
#
rails db:migrate

############
# check form
#
http://127.0.0.1:3000/posts/new



##################
# ActionText SETUP
#
vi app/models/post.rb
--
class Post < ApplicationRecord
  has_rich_text :content
end

--

vi app/views/posts/_form.html.erb 
--
<div class="field">
  <%= form.label :content %>
  <%= form.rich_text_area :content %>
</div>

--



```