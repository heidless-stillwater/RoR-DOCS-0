
# docs
- #### [gem: faker](https://github.com/faker-ruby/faker)
- #### [Getting Started with Minitest](https://semaphore.io/community/tutorials/getting-started-with-minitest)
- #### [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers)
- #### [Setup RSpec on a fresh Rails 7 project](https://dev.to/adrianvalenz/setup-rspec-on-a-fresh-rails-7-project-5gp)
- #### [How to Test Rails Models with RSpec](https://semaphore.io/community/tutorials/how-to-test-rails-models-with-rspec)

## [How to RSpec - Fairly comprehensive starter guide to RSpec](https://www.youtube.com/watch?v=BXaMRm1FDa8)


## quickref
```
# install rails
gem list rails
gem install rails -v <VERSION>


# list gems
gem list --local  # list all installed rails versions

# create app
rails _7.2_ new [PROJECT_NAME] -b bootstrap --database=postgresql
rails _6.1.7_ new [PROJECT_NAME] --css tailwind --database=postgresql

```

## gems
```
group :development, :test do
  gem 'factory_bot_rails'
  gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'main'
  gem 'rspec-rails', '~> 7.1', '>= 7.1.1'
end 

group :test do
  gem 'shoulda-matchers', '~> 6.0'
end

bundle install
rails g rspec:install

```