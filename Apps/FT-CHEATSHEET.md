# This is the finance tracker app from the Complete Ruby on Rails Developer course

# app info
- ## [udemy: fin-track](https://github.com/udemyrailscourse/finance-tracker-6)
- ## [github: fin-track](https://github.com/heidless-stillwater/fin-track)
- ## [github: financial-tracker - EXEMPLAR](https://github.com/heidless-stillwater/fin-track)

# [devise](https://github.com/heartcombo/devise)

# iex cloud
- ## [iex-ruby-client](https://github.com/dblock/iex-ruby-client)
- ## [website](https://iexcloud.io/console/home)
  - ## [iex: configure account](https://www.udemy.com/course/the-complete-ruby-on-rails-developer-course/learn/lecture/17657012#overview)
  - ## [iex-ruby-client](https://github.com/dblock/iex-ruby-client)
  - ## [RAILS: secure credentials](https://www.udemy.com/course/the-complete-ruby-on-rails-developer-course/learn/lecture/17657080#overview)

```
ACCOUNT:
rob.lockhart@yahoo.co.uk

heidlessemail05@gmail.com
$Ez4waZ2$w7Xjn#

STATUS:
Free Trial until 14 May

TOKEN-PUBLISHABLE:
pk_808439936a93476fb7558256a9b6da5c

TOKEN-SECRET:
sk_7de5c9cf9cb74ec3ad57523bc62cc986

# make sure to delete previous config/credentials.yml.enc
rm config/credentials.yml.enc config/master.key
---
# password value in file config/credentials.yml.enc
EDITOR='subl --wait' ./bin/rails credentials:edit
--
secret_key_base: GENERATED_VALUE
gcp:
  db_password: rgviFkldXdJyEwcNUoTpqfTtNagKEKTdpzlfWFYFUNURfiLdEA
iex_client:
  api_key: pk_808439936a93476fb7558256a9b6da5c
  secret_api_key: sk_7de5c9cf9cb74ec3ad57523bc62cc986
--

IEX::Api.configure do |config|
  config.publishable_token = 'pk_808439936a93476fb7558256a9b6da5c' # defaults to ENV['IEX_API_PUBLISHABLE_TOKEN']
  config.secret_token = 'sk_7de5c9cf9cb74ec3ad57523bc62cc986' # defaults to ENV['IEX_API_SECRET_TOKEN']
  config.endpoint = 'https://cloud.iexapis.com/v1' # use 'https://sandbox.iexapis.com/v1' for Sandbox
end

client = IEX::Api::Client.new(
  publishable_token: 'pk_808439936a93476fb7558256a9b6da5c',
  secret_token: 'sk_7de5c9cf9cb74ec3ad57523bc62cc986',
  endpoint: 'https://cloud.iexapis.com/v1'
)

```

# [Ruby on Rails - AJAX](https://www.tutorialspoint.com/ruby-on-rails/rails-and-ajax.htm)

# model
```
rails g model Stock ticker:string name:string last_price:decimal

rails db:migrate

rails c
--
my_stock = Stock.new(name: 'Alphabet', ticker: 'GOOG', last_price: 1300)
my_stock.save
Stock.all

--

```

# credentials
```
EDITOR='subl --wait' ./bin/rails credentials:edit
--
iex_client:
  api_key: pk_144dbfb0e9da41edb27e248f3c651b97
  secret_api_key: sk_3dfb88e7936c4247911eaff2e9200b2b
--

```
# ajax

- ## [How to Use AJAX with Rails](https://reintech.io/blog/how-to-use-ajax-with-rails)
- ## [Rails form_tag vs. form_for vs. form_with](https://medium.com/@michellekwong2/form-tag-vs-form-for-vs-form-with-fa6e0ac73aac)

- ## [XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- ## [Remote form submission and AJAX in Rails](https://dev.to/shehrozirfan/remote-form-submission-and-ajax-in-rails-15e6)


# photo-app

```
rails g scaffold Image name:string picture:string user:references

rails g bootstrap:themed Images

rails g uploader Picture

sudo apt-get install imagemagick --fix-missing


```


