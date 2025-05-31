

# [How to Test Rails Models with RSpec](https://semaphore.io/community/tutorials/how-to-test-rails-models-with-rspec)

#################
# INITIALIZE
#
```
rails _7.2.2.1_ new rspec_exemplar_v1 -j esbuild --css bootstrap --database=postgresql
rails db:prepare

```

######
# Gems
#
# RSpec Rails
# https://github.com/rspec/rspec-rails
```
bundle add rspec-rails --group "development, test"
bundle add factory_bot_rails --group "development, test"
bundle add faker

bundle add shoulda-matchers --group "development, test"
# spec/rails_helper.rb
--
...
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
--
bundle config set path 'vendor/bundle'
bundle install

```

#########
# Install
#
```
rails g rspec:install

# rails generate rspec:model post
# rails generate rspec:controller post

```

#########
# Example
#
```
rails generate model User \
                     email:string \
                     user:string \
                     password:string \
                     is_admin:boolean

rails generate model Auction \
                     start_date:datetime \
                     end_date:datetime \
                     title:string \
                     description:text \
                     user:references

rails g model Bid \
                bidder_id:integer \
                auction_id:integer \
                amount:integer

rake db:migrate db:test:prepare


############
# TEST - run

rspec --format documentation

rspec --format progess

rspec spec/example_1_spec.rb --format documentation 


```

##########################################
# Continuous Integraption (CI) : SEMAPHORE
#
- ### [signup: Semaphone](https://semaphore.io/)
```
Get Started->Signup with GitHub
--

--

Git->Commit->Publish Branch

REPO_ADDRESS=git@github.com:heidless-stillwater/rspec_exemplar_v1.git

Semaphore Home->AddProject
```




















###############
# SEED Data
#
```
# db/seed.rb
--

Post.destroy_all 

# create 20 posts
20.times do
  Post.create(
    title: Faker::Lorem.sentence(word_count: 3),
    body: Faker::Lorem.paragraph(sentence_count: 3)
  )
end
--
rails db:seed

```
