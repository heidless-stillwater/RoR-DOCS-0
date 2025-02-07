
rand(0..27)



sudo systemctl restart postgresql

pg_dump airbnb_app_1_development -F t > airbnb_app_1_development-pre-RES-0.tar

rails g model property:references user:references reservation_date

# google cloud storage
### [Using Google Cloud Storage API in Python For Beginners](https://www.youtube.com/watch?v=pEbL_TT9cHg)
- #### [enable: Google Cloud Storage JSON API](https://console.cloud.google.com/marketplace/product/google/storage-api.googleapis.com?hl=en-GB&inv=1&invt=Abmj3Q&project=h-pfolio-1)
- #### [enable: Cloud Storage](https://console.cloud.google.com/apis/library/storage-component.googleapis.com?inv=1&invt=Abmj3w&project=h-pfolio-1)


```


```


# app-framework
```
gem install rails -v 7.2.1

# [railsbytes: rails templates](https://railsbytes.com/)
APP_NAME=airbnb_rev_0
RAILS_BUILD_VERSION=7.2.1

# WITHOUT TESTS
rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} -T -d postgresql -cc tailwind

cd ${APP_NAME}

rails db:create

```

# rspec 
```
# install

gem search '^rspec-rails$' --all

gem install rspec-rails -v 7.1.0

vi Gemfile
--
group :development, :test do
  gem 'rspec-rails', '~> 7.0.0'
end
--

rails generate rspec:install

rspec ./spec/requests/home_spec.rb

```

# devise

vi Gemfile
```
gem 'devise'

rails g devise:install

rails g devise user

rails db:migrate db:test:prepare

rails g devise:views

```

# migrations
```
rails g controller Home index

rails g model property name:string headline:string description:text city:string state:string country:string

rails g migration add_address_columns_to_properties address_1:string address_2:string

rails g migration add_latitude_and_longitude_to_properties latitude:float longitude:float
--
add_index :properties, [:latitude, :longitude]
--


rails db:migrate db:test:prepare


```

# mapkick-link
### [Mapkick Gem Spotlight | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=hck_SWp1cVA)
- ### [mapkick:Create beautiful JavaScript maps with one line of Ruby](https://chartkick.com/mapkick)
```
########
# mapbox
https://docs.mapbox.com/api/maps/


#############
# CREDENTIALS
#
AccountID: 
rob.lockhart@yahoo.co.uk

password:
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
