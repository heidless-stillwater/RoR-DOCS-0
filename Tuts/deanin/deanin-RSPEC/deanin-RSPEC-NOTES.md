
# [deanin](https://deanin.com/)
- ## [Iterate Fast – How You Can Have Fewer Bugs By Using Rspec TDD In Ruby On Rails 6 ](https://deanin.com/blog/rspec-rails/)

---
---

# [RSpec TDD - How To Unit Test Ruby On Rails 6 Apps For Absolute Beginners](https://www.youtube.com/watch?v=AAqPc0j_2bg&t=121s)
```

gem install rails -v 6.1.7.7

export PATH="/home/heidless/.rvm/gems/ruby-3.2.2/bin:$PATH"

/bin/zsh --login && rvm use --default 3.2.2

# new app: '-t' = 'skip test files = '--skip-test'

rails _6.1.7.7_ new <APP_NAME> -d postgresql -t

```

# [railsbytes: rails templates](https://railsbytes.com/)
```
APP_NAME=deanin-rspec-tested-app-1
RAILS_BUILD_VERSION=6.1.7.7

rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -t

cd ${APP_NAME}

vi Gemfile
--
gem 'devise'

group :development do
  gem 'rspec-rails'
  ...
--
bundle install

# add 'RSpec' Templates
# https://railsbytes.com/public/templates/z0gsLX
#
rails app:template LOCATION="https://railsbytes.com/script/z0gsLX"
---
# remove 'test' directory: y

---

rails g devise:install
rails g devise User   # create devise Users

# IF NEEDED
# rails db:rollback

rails db:migrate RAILS_ENV=test

rails spec

# generate minimal app as example to create tests

rails g scaffold posts title:string body:text user:references views:integer


vi db/migrate/*_create_posts
--
      t.integer :views, default: 0

--
rails db:migrate RAILS_ENV=test


rspec spec

rspec spec --format documentation


```

# webpacker install
```
https://yarnpkg.com/lang/en/docs/install/

npm install --global yarn

rails webpacker:install

```

# view app
```
rails db:migrate RAILS_ENV=development

rails s

```

# init users
```


```



















# Reference
## [The basic structure (describe/it)](https://rspec.info/features/3-12/rspec-core/example-groups/basic-structure/)
## [before and after hooks](https://rspec.info/features/3-12/rspec-core/hooks/before-and-after-hooks/)
## [Module: RSpec::Matchers](https://www.rubydoc.info/gems/rspec-expectations/RSpec/Matchers)

-----

# Tutorials
## [rspec: tutorial - udemy](https://www.udemy.com/course/testing-ruby-with-rspec/learn/lecture/12418070#overview)
## [RSpec Tutorial for Beginners](https://www.youtube.com/watch?v=-uhFA74eBG0)
## [deanin: RSpec TDD - How To Unit Test Ruby On Rails 6 Apps For Absolute Beginners](https://www.youtube.com/watch?v=AAqPc0j_2bg&t=121s)

# GIT
## [udeny-rspec](https://github.com/heidless-stillwater/udeny-rspec)

# admin
```
gem install rspec

```

# new project
```
rspec --init


```

# run tests
```
# runs all tests in 'spec' folder
rspec

# run specific file
rspec spec/card_spec.rb



```
