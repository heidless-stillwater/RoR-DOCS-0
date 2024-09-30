
# BootStrap Config with Devise

## Gemfile
```
gem 'devise'
gem 'twitter-bootstrap-rails'
gem 'devise-bootstrap-views'
gem 'jquery-rails'

#bundle install --without production
bundle install

rails generate devise:install
rails generate devise User

```

## Migration File
```
## Confirmable
t.string   :confirmation_token
t.datetime :confirmed_at
t.datetime :confirmation_sent_at
t.string   :unconfirmed_email # Only if using reconfirmable

```

## user.rb
```
# add ':confirmable'

devise :database_authenticatable, :registerable, :confirmable,
        :recoverable, :rememberable, :validatable

```

## run migration
```
rails db:migrate

```

## application_controller.rb
```
before_action :authenticate_user!

```

## welcome_controller.rb
```
skip_before_action :authenticate_user!, only: [:index]

```

## run generators
```
rails generate bootstrap:install static
rails generate bootstrap:layout application # select Y to force override after hitting enter
rails generate devise:views:locale en
rails generate devise:views:bootstrap_templates

```

## app/assets/config/manifest.js
```
//= link application.js
//= link_tree ../images
//= link_directory ../stylesheets .css

```

## views/layouts/application.html.erb
```
, :skip_pipeline => true
```




