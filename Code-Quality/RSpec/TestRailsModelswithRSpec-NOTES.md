

## [How to Test Rails Models with RSpec](https://semaphore.io/community/tutorials/how-to-test-rails-models-with-rspec)

### init
```
rails _7.2.2.1_ new auctions -b bootstrap --database=postgresql

```

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

### model
```
bin/rails generate model Auction \
                     start_date:datetime \
                     end_date:datetime \
                     title:string \
                     description:text

rails db:prepare 

rails db:test:prepare
```

# validations
```
rspec spec/models/auction_spec.rb
```

# user model
```
rails generate model User \
  password:string  \
  email:string

rails db:migrate 
rails db:test:prepare

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

## bids
```
rails g model Bid \
                bidder_id:integer \
                auction_id:integer \
                amount:integer
rails db:migrate 
rails db:test:prepare

vi auction.rb
--
  has_many :bids, dependent: :destroy

--

vi bid.rb
--
  belongs_to :bidder, class_name: 'User'
  belongs_to :auction, class_name: 'Auction'

  validates_presence_of :bidder

--

#rails generate migration AddUserToBids user:references
#rails db:migrate

```
