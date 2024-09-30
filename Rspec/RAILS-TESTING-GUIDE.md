

# init project
```

# install
gem install rails 

/bin/zsh --login; rvm use --default 3.2.2


# [railsbytes: rails templates](https://railsbytes.com/)
```
APP_NAME=heidless-rspec-app-1
RAILS_BUILD_VERSION=7.2.1

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

```

rails g devise:install
rails g devise User   # create devise Users

# IF NEEDED
# rails db:rollback

rails db:migrate RAILS_ENV=test

rails spec
