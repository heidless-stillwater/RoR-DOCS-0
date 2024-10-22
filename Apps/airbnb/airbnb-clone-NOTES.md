
# [YOUTUBE: Let's build an Airbnb clone with Ruby on Rails - Part 1](https://www.youtube.com/watch?v=D889P37r3bc&list=PLCawOXF4xaJK1_-KVgXyREULRVy_W_1pe)

## [RSpec: AirBNB Clone: How to RSpec - Fairly comprehensive starter guide to RSpec](https://www.youtube.com/watch?v=AAqPc0j_2bg&t=121s)

### CONTENTS
- ### [git](#git-link)
- ### [rspec](#rspec-link)
- ### [cheatsheet](#cheatsheet-link)
- ### [moneytize](#monetize-link)
- ### [active-storage](#active-storage-link)
- ### [active-record](#active-record-link)
- ### [reviews](#reviews-link)
- ### [mapkick](#mapkick-link)
- ### [geocoder](#geocoder-link)
- ### [social-signup](#social-signup-link)
- ### [bootstrap](#bootstrap-link)
- ### [axios](#axios-link)
- ### [mdb](#mdb-link)
- ### [factory-bot](#factory-bot-link)

- ### [TROUBLESHOOT](#troubleshoot-link)
- ### [app-framework](#app-framework-link)
- ### [devise](#devise-link)
- ### [signup-modal](#signup-modal-link)
- ### [webpacker](#webpack)
- ### [tailwindcss](#tailwindcss-link)

---
# git-link
```
git checkout main

export H_BRANCH='19-pricing-1'
echo ${H_BRANCH}
git branch -d ${H_BRANCH}           # Delete local
git push -d origin ${H_BRANCH}      # Delete remote

## view commit history
git log
--
commit d6d2da4b4fdc2eab1da942f2f64a2c6b053a2d5a (HEAD -> 3-modal-4, origin/DEVELOP-0, DEVELOP-0)

--

## Reverting Working Copy to Most Recent Commit
## To revert to the previous commit, ignoring any changes:

git reset --hard HEAD

git reset --hard d6d2da4b4fdc2eab1da942f2f64a2c6b053a2d5a

# where HEAD is the last commit in your current branch

```

# rspec-link
### [shoulda-matchers](https://github.com/thoughtbot/shoulda-matchers)
### [Rails 6.1 adds support for validating numeric values fall in a range using `in:` option](https://blog.saeloun.com/2021/02/05/add-validate-numericality-in-range/)
```

rspec --init      # sets up a base skeleton for RSpec testing in the current app

rspec   # run ALL spec files

rspec spec --format documentation     # run ALL tests with DETAIL of both SUCCESS & FAILURE

rspec spec/models/post_spec.rb --format documentation 
rspec spec/models/property_spec.rb --format documentation 
rspec spec/models/profile_spec.rb --format documentation 
rspec spec/requests/home_spec.rb --format documentation 
rspec spec/requests/api/users_spec.rb --format documentation 

rspec spec/requests/api/users_by_email_spec.rb --format documentation 

rspec spec/views/posts/index.html.erb_spec.rb --format documentation 

rspec spec/card_spec.rb   #  isolate SPECIFIC test

./spec/card_spec.rb:3   # run spec file from example with LINE NUMBER = 3

# install
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

# cheatsheet-link
```
rvm use --default 3.3.5

/bin/zsh --login && rvm use --default 3.3.5

gem install rails -v 7.2.1

rails routes
rails routes -c api/users
rails routes -c api/users_by_email

# to fix deprecation warnings upgrade sass
#npm i sass@1.77.6 --save-exact
#rails _7.2.1_ new airbnb-app-2 -T -d postgresql -j esbuild --css bootstrap

#rails _7.2.1_ new airbnb-bootstrap-0 -d postgresql -j esbuild --css bootstrap

rails _7.2.1_ new partials-test-0 -d postgresql

gem uninstall gem-wrappers


#rails _6.1.7.8_ new test-app-2 -d postgresql

###########
# [bootstrap](https://getbootstrap.com/docs/5.0/getting-started/introduction/)
###########

rails g controller Home index

rails g model property name:string headline:string description:text city:string state:string country:string

rails g migration add_address_columns_to_properties address_1:string address_2:string

rails g migration add_latitude_and_longitude_to_properties latitude:float longitude:float
--
add_index :table, [:latitude, :longitude]
--
rails db:migrate db:test:prepare

psql
--
ALTER TABLE properties
DROP COLUMN longitude;

--
    return "301 Park Ave, New York, NY 10022, United States"
    return "City Centre, First Avenue, Lebuh Bandar Utama, Bandar Utama, 47800 Petaling Jaya, Selangor, Malaysia"
    # return "Aldwych, London WC2B 4DD"
--

# google maps
rails generate stimulus google-map

# places
rails generate stimulus maps
rails g scaffold places name latitude:decimal longitude:decimal

#########
# profile
#
rails g model profile user:references address_1:string address_2:string city:string state:string country:string latitude:float longitude:float

rails g migration add_zip_code_to_profiles zip_code:string
rails g migration add_zip_code_to_properties zip_code:string

rails db:migrate db:test:prepare

rails c
--
new_user = User.create email: "foo_1@bar.com", password: "password"
profile = new_user.profile
profile.update address_1: "99 Bd Haussmann, 75008 Paris, France", city: "Paris", state: "Île-de-France", country: "France", zip_code: "75008"


user = User.last
profile = user.profile
profile.update address_1: "99 Bd Haussmann, 75008 Paris, France", city: "Paris", state: "Île-de-France", country: "France", zip_code: "75008"

```

# monetize-link
```
rails g migration add_price_cents_to_properties 
--
def change
  add_monetize :properties,
               :price,
               amount: { null: true, default: nil },
               currency: { null: true, default: nil }
end

--
prop.price = Money.from_amount(50, "USD")

Property.find_each do |property|
      property.price = Money.from_amount((20..100).to_a.sample, "USD")
      property.save!
end
Property.pluck(:price_cents)

```

# active-storage-link
### [Active Storage Overview](https://guides.rubyonrails.org/active_storage_overview.html)
```
Gemfile: uncomment "image_processing"

rails active_storage:install
rails db:migrate db:test:prepare

rails g migration add_images_to_properties images:attachments

```

# active-record-link
### [Active Storage Basics](https://edgeguides.rubyonrails.org/active_storage_overview.html)
```
tester

```

# reviews-link
### [Active Record Associations](https://guides.rubyonrails.org/association_basics.html)
```
rails g model review 
vi <MIGRATION>
--
  def change
    create_table :reviews do |t|
      t.string  :title
      t.text    :body
      t.integer :rating
      t.bigint  :reviewable_id
      t.string  :reviewable_type
      t.timestamps
    end

    add_index :reviews, [:reviewable_type, :reviewable_id]

  end

--
rails db:migrate db:test:prepare

```

# mapkick-link
### [Mapkick Gem Spotlight | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=hck_SWp1cVA)
- ### [mapkick:Create beautiful JavaScript maps with one line of Ruby](https://chartkick.com/mapkick)
```
#############
# CREDENTIALS
#

ACCESS:
mapbox PWD:
--
ciyJz.&_$3k!!ip
--
access-token-0
pk.eyJ1IjoiaGVpZGxlc3MiLCJhIjoiY20yZ2Zvc3hoMDByMzJqc2d1MWozbGJyaCJ9.dQLSI42M1Eo7wnXXzbjipA

EDITOR='subl --wait' ./bin/rails credentials:edit
--
mapkick: pk.eyJ1IjoiaGVpZGxlc3MiLCJhIjoiY20yZ2Zvc3hoMDByMzJqc2d1MWozbGJyaCJ9.dQLSI42M1Eo7wnXXzbjipA

--

vi ./config/initializers/mapkick.rb
--
ENV["MAPBOX_ACCESS_TOKEN"] = Rails.application.credentials.dig(:mapkick)

--
```

# geocoder-link
### [geocoder config](https://github.com/alexreisner/geocoder)
- ### generate a config file with the http_header set
```
rails generate geocoder:config
--
Geocoder.configure(http_headers: { "User-Agent" => "your contact info" })

--

Property.last.update! latitude: nil, longitude: nil

```

# social-signup-link
### [social signup page](https://www.creative-tim.com/bits/bootstrap/signup-page-now-ui-kit)

### [16. Login/Registration Form Transition (Nikolay Talanov on CodePen)](https://blog.hubspot.com/website/bootstrap-form-template)

### [Login Form 4 by Colorlib](https://colorlib.com/wp/template/login-form-v4/)

### [Bootstrap Signup Form Template](https://bootstrapbrain.com/demo/components/registrations/registration-7/)

# bootstrap-link
### [install bootstrap](https://mixandgo.com/learn/ruby-on-rails/how-to-install-bootstrap)
```
bundle add cssbundling-rails
./bin/rails css:install:bootstrap

bundle add jsbundling-rails
./bin/rails javascript:install:esbuild

```

# axios-link
### [axios :: JavaScript libaries, Vue.js, Ruby on Rails and importmaps](https://jasonchee.me/writings/javascript-libraries-vue-and-importmaps/)
```
vi config/importmap.rb
--
pin "axios", to: "https://cdn.skypack.dev/pin/axios@v1.6.7-CtiTWk1RHZKnFCXj0sDG/mode=imports,min/optimized/axios.js", preload: true

--
```

# mdb-link
### [mdb](https://mdbootstrap.com/docs/standard/navigation/navbar/examples-and-customization/)
### [mdb: installation](https://mdbootstrap.com/docs/standard/extended/registration/)
### [Login PopUp](https://mdbootstrap.com/docs/standard/extended/login/)
### [mdb: cdn](https://mdbootstrap.com/docs/standard/extended/registration/)
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


# factory-bot-link
### [factory_bot](https://github.com/thoughtbot/factory_bot)
```
bundle add factory_bot

```

# troubleshoot-link
```
#
# PROBLEM
#
rake aborted!
Step #0 - "build image": SassC::SyntaxError: Error: Invalid CSS after "}": expected 1 selector or at-rule, was '<link rel="styleshe' (SassC::SyntaxError)
Step #0 - "build image":         on line 42:2 of stdin
Step #0 - "build image": >> }

#
# SOLUTION
#
vi config/application.rb
--
config.assets.css_compressor = nil

--
```

# app-framework-link
```
# [railsbytes: rails templates](https://railsbytes.com/)
APP_NAME=airbnb-app-1
RAILS_BUILD_VERSION=7.2.1

# WITHOUT TESTS
rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -T -d postgresql -cc tailwind

cd ${APP_NAME}

rails db:create


```

# devise-link
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

# signup-modal-link
### [signup modal](https://auth.freshbooks.com/service/auth/integrations/sign_in?_gl=1*y5hk91*_gcl_au*MTg5MjcyNjY1NC4xNzExOTc4MDAx*_ga*MTc4MDg0MTQ4OS4xNjg5NzcyNDcy*_ga_LNDHWTHSMK*MTcxOTU3ODM5Ni4yNTkuMC4xNzE5NTc4Mzk2LjYwLjAuMA..)


# tailwindcss-link
### [tailwindcss](https://tailwindcss.com/docs/installation)
```
npm install -D tailwindcss
npx tailwindcss init

```
