

## [usemy: testing ruby with RSpec](https://www.udemy.com/course/testing-ruby-with-rspec/learn/lecture/12409324#overview)

### initialize app
```
#RAILS_BUILD_VERSION=6.1.7.7
#RAILS_BUILD_VERSION=7.2.2.1
#gem install rails -v ${RAILS_BUILD_VERSION}
#export PATH="/home/heidless/.rvm/gems/ruby-3.2.2/bin:$PATH"
#/bin/zsh --login

rvm use --default 3.2.2

# new app: '-t' = 'skip test files = '--skip-test'

APP_NAME=heidless-rspec-app-1
rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -d postgresql -t

# initialize rspec
rspec --init

```