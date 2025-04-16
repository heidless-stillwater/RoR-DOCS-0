
### Authentication
  - ### [devise](https://github.com/heartcombo/devise)


# install
```
bundle add devise

rails generate devise:install
#rails g devise:views

```

# create User
```
rails generate devise User
rails routes|grep devise

rails db:migrate

# restart server
rails s

vi config/environments/develpment.rb
--
config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }

--

vi controllers/application_controller.rb
--
before_action :authenticate_user!

--

```

# create bootstrap styled views
- ### [devise-bootstrap-views](https://github.com/hisea/devise-bootstrap-views)
```
vi Gemfile
--
bundle add devise-bootstrap-views

--

rails generate devise:views:bootstrap_templates

```
