
### [YOUTUBE: Let's build an Airbnb clone with Ruby on Rails - Part 1](https://www.youtube.com/watch?v=D889P37r3bc&list=PLCawOXF4xaJK1_-KVgXyREULRVy_W_1pe)

### [RSpec: AirBNB Clone: How to RSpec - Fairly comprehensive starter guide to RSpec](https://www.youtube.com/watch?v=AAqPc0j_2bg&t=121s)

[TOC]

## CONTENTS
- ### [cheatsheet](#cheatsheet)
- ### [app framework](#appframework)
- #### [webpacker](#webpack)
- ### [gem init](#geminit)
- ### [railsbytes](#railsbytes-template)
- ### [devise](#devise-gem)
- ### [minimal app](#minimalapp)

## CHEATSHEET
### env     
```
rvm use --default 3.2.2
/bin/zsh --login

rails routes

rails routes -c api/users

# to fix deprecation warnings upograde sass
#npm i sass@1.77.6 --save-exact
#rails _7.2.1_ new airbnb-app-2 -T -d postgresql -j esbuild --css bootstrap

rails _7.2.1_ new airbnb-app-3 -d postgresql -css bootstrap


rails _6.1.7.8_ new test-app-2 -d postgresql

bundle add ostruct

rails g controller Pages index


```

### [social signup page](https://www.creative-tim.com/bits/bootstrap/signup-page-now-ui-kit)

### [16. Login/Registration Form Transition (Nikolay Talanov on CodePen)](https://blog.hubspot.com/website/bootstrap-form-template)

### [Login Form 4 by Colorlib](https://colorlib.com/wp/template/login-form-v4/)

### [Bootstrap Signup Form Template](https://bootstrapbrain.com/demo/components/registrations/registration-7/)

### []()

```


```



### [mdb](https://mdbootstrap.com/docs/standard/navigation/navbar/examples-and-customization/)
```
# Login PopUp
https://mdbootstrap.com/docs/standard/extended/login/


```

### [factory_bot](https://github.com/thoughtbot/factory_bot)
```
bundle add factory_bot

```

### rspec
```

rspec --init      # sets up a base skeleton for RSpec testing in the current app

rspec   # run ALL spec files

rspec spec --format documentation     # run ALL tests with DETAIL of both SUCCESS & FAILURE

rspec spec/models/post_spec.rb --format documentation 
rspec spec/requests/users_spec.rb --format documentation 
rspec spec/requests/home_spec.rb --format documentation 
rspec spec/requests/api/users_by_email_spec.rb --format documentation 

rspec spec/views/posts/index.html.erb_spec.rb --format documentation 

rspec spec/card_spec.rb   #  isolate SPECIFIC test

./spec/card_spec.rb:3   # run spec file from example with LINE NUMBER = 3

```
### git
```
git checkout main

export H_BRANCH='DEPLOY-0'
echo ${H_BRANCH}
git branch -d ${H_BRANCH}               # Delete local
git push -d origin ${H_BRANCH}   # Delete remote

## view commit history
git log
--
commit d6d2da4b4fdc2eab1da942f2f64a2c6b053a2d5a (HEAD -> 3-modal-4, origin/DEVELOP-0, DEVELOP-0)

--

## Reverting Working Copy to Most Recent Commit
## To revert to the previous commit, ignoring any changes:
## git reset --hard HEAD

git reset --hard d6d2da4b4fdc2eab1da942f2f64a2c6b053a2d5a

# where HEAD is the last commit in your current branch

```
---

## AppFramework
```
# [railsbytes: rails templates](https://railsbytes.com/)
APP_NAME=airbnb-app-1
RAILS_BUILD_VERSION=7.2.1

# WITHOUT TESTS
rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -T -d postgresql -cc tailwind

cd ${APP_NAME}

rails db:create


```

## rspec
```
vi Gemfile
--
group :development, :test do
  gem 'rspec-rails'
  ...
--
bundle install

rails generate rspec:install
      create  .rspec
      create  spec
      create  spec/spec_helper.rb
      create  spec/rails_helper.rb

rspec spec --format documentation


```

# devise
```
vi Gemfile
--
gem 'devise'

--
bundle install

rails g devise:install
rails g devise user  # create devise user model

# IF NEEDED
# rails db:rollback

rails db:migrate db:test:prepare

rails g devise:views

```

# (M)aterial (D)esign (B)ootstrap (MDB)
## [mdb: installation](https://mdbootstrap.com/docs/standard/extended/registration/)
```
npm install -g mdb-cli

mdb register 
--
name: 
rob

username:
heidless

email: 
lockhart.r@gmail.com

password:
Havana111065
            
--

mdb login

# initialize a project
mdb frontend init mdb5-free-standard

mdb frontend init mdb5-free-standard

cd mdb5-free-standard
npm install

```

## [mdb: cdn](https://mdbootstrap.com/docs/standard/extended/registration/)
```

```

# [signup modal](https://auth.freshbooks.com/service/auth/integrations/sign_in?_gl=1*y5hk91*_gcl_au*MTg5MjcyNjY1NC4xNzExOTc4MDAx*_ga*MTc4MDg0MTQ4OS4xNjg5NzcyNDcy*_ga_LNDHWTHSMK*MTcxOTU3ODM5Ni4yNTkuMC4xNzE5NTc4Mzk2LjYwLjAuMA..)


# [tailwindcss](https://tailwindcss.com/docs/installation)
```
npm install -D tailwindcss
npx tailwindcss init

```


























###############################################
###############################################
###############################################
## webpack
```
https://yarnpkg.com/lang/en/docs/install/

npm install --global yarn

rails webpacker:install

```

## railsbytes-template
### https://railsbytes.com/public/templates/z0gsLX
# install RSpec template
```
rails app:template LOCATION="https://railsbytes.com/script/z0gsLX"
---
# remove 'test' directory: y
```

## devise-gem
```

rails g devise:install
rails g devise User   # create devise Users

# IF NEEDED
# rails db:rollback

rails db:migrate RAILS_ENV=test


```

## MinimalApp
```
rails g scaffold posts title:string body:text user:references views:integer

vi db/migrate/*_create_posts
--
      t.integer :views, default: 0

--
rails db:migrate RAILS_ENV=test

rspec spec --format documentation

rails db:migrate RAILS_ENV=development

rails s

```

