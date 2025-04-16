
# docs
- #### [gem: faker](https://github.com/faker-ruby/faker)
- #### [Getting Started with Minitest](https://semaphore.io/community/tutorials/getting-started-with-minitest)
- #### [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers)
- #### [Setup RSpec on a fresh Rails 7 project](https://dev.to/adrianvalenz/setup-rspec-on-a-fresh-rails-7-project-5gp)
- #### [How to Test Rails Models with RSpec](https://semaphore.io/community/tutorials/how-to-test-rails-models-with-rspec)

### ref
- ### [A Guide to Testing Rails Applications](https://guides.rubyonrails.org/v2.3/testing.html)
- ### [home: RSpec Rails](https://rspec.info/features/6-0/rspec-rails/)
- ### [github: rspec-rails](https://github.com/rspec/rspec-rails)
- ### [API Docs: Module: RSpec](https://rubydoc.info/github/rspec/rspec-core/RSpec)
- ### [Rails Testing Handbook](https://semaphore.io/ebooks/rails-testing-handbook)

### training
- ### [Using Test-Driven Development to Understand the MVC in Ruby on Rails](https://www.microverse.org/blog/using-test-driven-development-to-understand-the-mvc-in-ruby-on-rails)
- ### [8 Beautiful Ruby on Rails Apps in 30 Days & TDD - Immersive](https://www.udemy.com/course/8-beautiful-ruby-on-rails-apps-in-30-days/)
- ### [Professional Rails Testing: Tools and Principles](https://www.amazon.com/Professional-Rails-Testing-Tools-Principles/dp/B0DJRLK93M#customerReviews)
- ### RSpec - Medium
  - ### [RSpec Rails, Part 2](https://davidrmorphew.medium.com/rspec-rails-part-2-b2f09b85aec4)


### syntax/tools
- ### [Explicit Subject](https://rspec.info/features/3-12/rspec-core/subject/explicit-subject/)
- ### [described_class](https://rspec.info/features/3-13/rspec-core/metadata/described-class/)


### info
- #### [Getting Started with Minitest](https://semaphore.io/community/tutorials/getting-started-with-minitest)

###########
# Quick Ref
#
```
'let'   # lazy loading. created upon first invocations
'let!'  : non-lazy loading. created immediately

# before & after hooks
#
before :suite
before :context
before :example
after  :example
after  :context
after  :suite

# useful matchers#
#
include (for arrays & hashes)
start_with
end_with
match (for regular expression matching)
be_between
have_key / have_value (for hashes)
be_instance_of
respond_to
have_attributes (for testing instance variables)

# raise_error matcher   (wrap expectation in a block)
expect{ :x.count }.to raise_error(NoMethodError)

# RSpec Formatters
#
rspec spec/example_1_spec.rb --format progress 
rspec spec/example_1_spec.rb --format documentation 
rspec spec/example_1_spec.rb --format json 
rspec spec/example_1_spec.rb --format html 

# DB
rails db:prepare

rails db:migrate
rails db:rollback STEP=1

rails db:seed

rails db:test:prepare

rails generate rspec:model User

rails generate rspec:controller MyController

```

## associations
```
class User < ApplicationRecord
  has_many :auctions, dependent: :destroy
end

class Auction < ApplicationRecord
  belongs_to :users
end

rails generate migration AddUserToAuctions user:references
rails db:migrate

###############

rails g model Category name:integer

rails db:migrate 
rails db:test:prepare



```


#########
### RSpec
```
#######
# RSpec
#
vi Gemfile
--
group :development, :test do
  gem 'rspec-rails', ">= 3.9.0"
end
--
bundle config set path 'vendor/bundle'
bundle install
bin/rails generate rspec:install
#bin/rails webpacker:install 

# list gems & versions installed
bundle list

```

## RSpec Configure
```
group :development, :test do
  gem 'rspec-rails', git: 'https://github.com/rspec/rspec-rails'
end

```

## should matchers
```
vi Gemfile
--
group :development, :test do
  gem 'shoulda-matchers'
end
--

vi spec/models/auction_spec.rb
--
describe "Associations" do
  it { should belong_to(:user).without_validating_presence }
end
--

vi spec/rails_helper.rb
--
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
--

```

## run
```
rspec

rspec spec

rspec spec --format documentation
rspec spec/card_spec.rb

# RSpec Formatters
#
rspec spec/example_1_spec.rb --format progress 
rspec spec/example_1_spec.rb --format documentation 
rspec spec/example_1_spec.rb --format json 
rspec spec/example_1_spec.rb --format html 

```

##############
# init project
```

# install
gem install rails 

/bin/zsh --login; rvm use --default 3.2.2

```

# [railsbytes: rails templates](https://railsbytes.com/)
```
APP_NAME=heidless-rspec-app-1
RAILS_BUILD_VERSION=7.2.1

rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -t

#rails new deploy_test_0 -b bootstrap --database=postgresql
#rails new rails_alpha_blog --css tailwind --database=postgresql

cd ${APP_NAME}

# add 'RSpec' Templates
# https://railsbytes.com/public/templates/z0gsLX
#
rails app:template LOCATION="https://railsbytes.com/script/z0gsLX"

```


#####################################
# instal Devise & configure for RSpec
#
rails g devise:install
rails g devise User   # create devise Users

# IF NEEDED
# rails db:rollback

rails db:migrate RAILS_ENV=test

rails spec
