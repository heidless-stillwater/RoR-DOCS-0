
## new app

### [How to use Bootstrap with CSS bundling in Rails 7](https://gorails.com/episodes/bootstrap-css-bundling-rails)
- ### [YOUTUBE: How to use Bootstrap with CSS bundling in Rails 7](https://www.youtube.com/watch?v=nxKDTtuKOo4)
```
# new app
rails _7.2.2.1_ new rails-fin-track-7 -j esbuild --css bootstrap --database=postgresql

# prepare database
rails db:prepare

# create minimal app
rails g controller welcome index

rails g bootstrap:install static 


#rails g bootstrap:layout application
rails g devise:views:locale en
rails g devise:views:bootstrap_templates


```
### custom.css.scss
Allow over-ride of Bootstrap elements
```
#########
# Gemfile
#
bundle add sass-rails

############################
# initialize custom.css.scss
#
vi app/assets/stylesheets/custom.css.scss
--
// app/assets/stylesheets/application.bootstrap.scss loaded with sprockets-rails
@import 'bootstrap/scss/bootstrap';

h1 {
  color: green;
}

.navbar-brand {
  font-size: 3rem;
}

--

#############
# manifest.js
#
vi app/assets/config/manifest.js
--
...
//= link custom.css
--

######################
# application.html.erb
#
vi app/views/layouts/application.html.erb
--
<%= stylesheet_link_tag "custom", "data-turbo-track": "reload" %>
--

###############################
# initializer
#
vi config/initializers/assets.rb
--
...
  Rails.application.config.assets.paths << Rails.root.join("node_modules/bootstrap-icons/font")
  Rails.application.config.assets.paths << Rails.root.join('node_modules')
--

##############
# start server
./bin/dev
#
#rails s

```

# ['navbar' to test bootstrap](https://getbootstrap.com/docs/5.3/components/navbar/)
```
# dropdown may now work if started with
rails s

# instead start with
./bin/dev

```


####################################
# create & initialise bootstrap(CDN)
```
rails _7.2.2.1_ new rails-fin-track-7 --database=postgresql

vi application.html.erb
--
<head>
...
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" crossorigin="anonymous">
...
</head>

<body>
...
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.12.9/dist/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
</body>

--

