
# [Intro Ruby on Rails 7 For Beginners Tutorial Series](https://www.youtube.com/playlist?list=PL3mtAHT_eRezB9fnoIcKS4vYFjm23vddb)

## [0: Intro to Ruby on Rails 7 Fullstack Tutorial](https://www.youtube.com/watch?v=TlgSp2XPCY4&list=PL3mtAHT_eRezB9fnoIcKS4vYFjm23vddb&index=1)

# install
```
# uninstall
#gem uninstall rails
#gem uninstall railties

#gem uninstall rails -v 6.1.7.7
#gem uninstall railties -v 6.1.7.7

--------------
# install
gem install rails 

#gem install rails -v 6.1.7.7
#rails  _6.1.7.7_ new rails-test-0 -d postgresql

#/bin/zsh --login
#export PATH="/home/heidless/.rvm/gems/ruby-3.2.2/bin:$PATH"
#rvm use --default 3.2.2

```

# init project
```
#
# rails new blog_demo -b bootstrap

APP_NAME=heidless-blog-demo-app-0
RAILS_BUILD_VERSION=7.2.1

#rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -t --database=postgresql -b bootstrap

rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -t -b bootstrap

cd ${APP_NAME}

vi Gemfile
--
gem 'devise'

group :development do
  gem 'rspec-rails'
  ...
--
bundle install






rails new blog_demo

rails g controller pages home about 

rails s   
--
http://127.0.0.1:3000/pages/home

--

```

# git - WIP Further practice needed
```
git checkout -b posts-0

# deal with untracked files
git status
git add .
git commit -m "add posts"
git push
--
git push --set-upstream origin posts-0

Github->'deanin-0-blog_demo'->Compare&PullRequest
--
add Description

--

->CreatePullRequest

->FilesChanged
--
click 'Review changes' if you apprive of the changes...

LGTM : Loogs Good To Me

--
->SubmitReview

->MergePullRequest
->ConfirmMerge
->Delete Branch

git checkout main

git fetch

git status
# your branch is behing by 2 commits

# pull changes
git pull

--
###########################################################
# cheatsheets
#
# discard edits - unstaged local changes
git status
git reset --hard





```

# blog posting
```
rails g scaffold post title body:text
rails db:migrate

rails c
--
@post = Post.new(title: "Hello World!", body: "This is my first post...")
@post
@post.save
@post
@post = Post.create(title: 'Second post!', body: 'This is my second post...')

@post = Post.find(2)

@post.title
Post.first

# find the last 2 posts
Post.last(2)

--

vi db/seed.rb
--
10.times do |x|
  Post.create(title: "Titles #{x}", body: "Body #{x} text here...")
end

--

rails db:seed

rails g migration add_views_again__to_posts views:integer

```

# remove column
```
#rails generate migration RemoveColumnViewFromPosts
--
#class RemoveColumnViewFromPosts < ActiveRecord::Migration
#  def change
#    remove_column :posts, :views
#  end
#end
--
```

# add views columns
```
# generate migration to add views column

rails g migration add_views_to_posts views:integer

vi db->migrate->*_add_views_to_posts
--
  # add 'default: 0'
  add_column :posts, :views, :integer, default: 0

--

```
---
---
# [DEVISE GEM](https://github.com/heartcombo/devise)
```
bundle add devise

rails generate devise:install

rails generate devise User

rails db:migrate

```

## generate user
```
rails g devise User

rails db:migrate

```

## routes.rb
```
vi config/routes.rb
--
devise_for :users

--

```

## post user
```
rails g migration add_user_to_posts user:belongs_to
rails c
--
Post.destroy_all

--

rails db:migrate

vi db/seeds.rb
--
User.create(email: "heidlessemail05@gmail.com", password: "password", password_confirmation: "password" )

10.times do |x|
  Post.create(title: "Titles #{x}", body: "Body #{x} text here...", user_id: User.first.id)
end

--

```

## add user_name
```
rails g migration add_name_to_user name:string

rails db:migrate

```

## devis views
```
rails g devise:views

```

## devise controller : users
```
rails g devise:controllers users

```

## users controllers
```
rails g controller users profile

rails g migration add_views_to_user views:integer

raild db:migrate

rails g migration change_views_for_users 
vi db/migrate/*_change_views_for_users.rb
--
  def change
    change_column :users, :views, :integer, default: 0
  end

--

## devise specs : generate devise tests
bin/rails generate devise:specs User


```
