
## resources
- ### [RubyOnRails](https://rubyonrails.org/)
  - ### [RoR-Guides](https://guides.rubyonrails.org/v6.1/)
  - ### [Understanding Ruby Series' Articles](https://dev.to/baweaver/series/11177)
    - [Understanding Ruby - Triple Equals](https://dev.to/baweaver/understanding-ruby-triple-equals-2p9c)
  - ### ActiveRecord
    - ### [Active Record Validations](https://guides.rubyonrails.org/active_record_validations.html)
    - ### [Active Record Associations](https://guides.rubyonrails.org/association_basics.html)
    - ### [Action View Form Helpers](https://guides.rubyonrails.org/form_helpers.html)
  - ### [form helpers](https://guides.rubyonrails.org/form_helpers.html)
  - ### [How to manage multiple Rails versions](https://medium.com/@relativkreativ/how-to-manage-multiple-rails-versions-bf5759d0823b)
  - ### Language References
    - ### [Inject Method: Explained](https://medium.com/@terrancekoar/inject-method-explained-ed531eff9af8)
  - ### UI
    - ### bootstrap
      - ### [Bootstrap themes, templates, and UI tools](https://startbootstrap.com/)
    - ### tailwind
      - ### [Tailwind CSS Alerts - Flowbite](https://flowbite.com/docs/components/alerts/)
      - ### [ReadyMadeUI - tailwind](https://readymadeui.com/)
    - ### [NavBar](https://readymadeui.com/tailwind-components/header)
  - ### [TW Elements open-source UI Kit](https://tw-elements.com/)
    - ### [TW Navbar - Configuration](https://tw-elements.com/docs/standard/navigation/navbar/)
    - ### [TW - javascript](https://tw-elements.com/learn/te-foundations/tailwind-css/javascript/#:~:text=As%20the%20name%20suggests%2C%20Tailwind,offer%20solutions%20related%20to%20it.)
- ### AWS
  - ### [Deploying a rails application to Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ruby-rails-tutorial.html)
- ### Authentication
  - ### [devise](https://github.com/heartcombo/devise)
- ### [will_paginate gem](https://github.com/mislav/will_paginate)
- ### [Rubular: Ruby regular expression editor](https://rubular.com/)
- ### [replit (github login)](https://replit.com/login)
- ### [rubygems](https://rubygems.org/)
- ### [Unsplash: free images](https://unsplash.com/)
- ### Ruby Style Guides
  - ### [Ruby Style Guide](https://rubystyle.guide/)
  - ### [rspec-style-guide](https://github.com/rubocop/rspec-style-guide)
  - ### [git: ruby-style-guide](https://github.com/rubocop/ruby-style-guide)
- ### [github.io TryRuby](https://try.ruby-lang.org/)

###################
# media links
- ### [LOGOS: WorldVectorLogo](https://worldvectorlogo.com/)

###################
# Active Record - subfolders (by tenant - Organization)
- ### [How to isolate your Rails blobs in subfolders](https://dev.to/drnic/how-to-isolate-your-rails-blobs-in-subfolders-1n0c)

###################
# Passing Params in Rails
- ### [Passing Params in Rails](https://medium.com/@lh62594/passing-params-in-rails-75dcb94470c4)
```
#####################################################
# Passing Params Through a Hidden Field in a form_tag
#


##################################
2. Passing Params Inside a link_to
#
<%= link_to link-name, path(params) %>

<%= link_to link-name, path(params: {param_1: value_1, ...}) %>


####################################
3. Passing Params Inside a button_to
#






```



```
# config/initializers/active_storage.rb
--
Rails.configuration.to_prepare do
  ActiveStorage::Blob.class_eval do
    before_create :generate_key_with_prefix

    def generate_key_with_prefix
      self.key = if prefix
        File.join prefix, self.class.generate_unique_secure_token
      else
        self.class.generate_unique_secure_token
      end
    end

    def prefix
      @org_active
    end
  end
end
--

```

###################
# Active Record - tenant (Organization) specific upload
#
```
# app/models/artifact.rb
--
  private

  def rename_with_tenant_upload_gcs
    this_org = @org_active
    blob = artimg.blob
    current_object_key = blog.key
    new_object_key = "#{this_org}/#{current_object_key}" # Construct the new object key
    blob.upload(new_object_key)
  end

--



```



# flatpickr
- ### [flapickr HomePage](https://www.youtube.com/watch?v=tww-0bO4zCY)
- ### [Flatpickr Date Picker in Ruby on Rails 7](https://www.youtube.com/watch?v=tww-0bO4zCY)
```
bin/importmap pin flatpickr

vi app/views/application.html.erb
--
...
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flatpickr/dist/flatpickr.min.css">
<script src="https://cdn.jsdelivr.net/npm/flatpickr"></script>
...
--

```


<hr>
<hr>

```
# toggle sidebar visibility
<ctrl>-b <ctrl>-b

```
<br />
<br />

## [The Complete Ruby on Rails Developer Course](https://www.udemy.com/course/the-complete-ruby-on-rails-developer-course/learn/quiz/4350012#overview)


### quickref
```
# install rails
gem list rails
gem install rails -v <VERSION>


# list gems
gem list --local  # list all installed rails versions

# create app
#rails _7.2.2.1_ new [PROJECT_NAME] -b bootstrap --database=postgresql


```

################
# INITIALISE App
#
### bootstrap & postgres
```
rails _7.2.2.1_ new rails-boostrap-exemplar -j esbuild --css bootstrap --database=postgresql

rails _7.2.2.1_ new gemeni-git-test -j esbuild --css bootstrap --database=postgresql

rails _7.2.2.1_ new video_api  --api --database=postgresql

# rails db:drop 

rails db:prepare

#######
# RSpec
#
# RSpec Rails
# https://github.com/rspec/rspec-rails

bundle add rspec-rails --group "development, test"
bundle add factory_bot_rails --group "development, test"
bundle add faker

#########
# Install
#
rails g rspec:install

rails generate rspec:model post
rails generate rspec:controller post

drop database react_on_rails_api_development;
drop database react_on_rails_api_test;

drop database react_on_rails_api_v3_development;
drop database react_on_rails_api_v3_test;

drop database react_on_rails_api_v1_development;
drop database react_on_rails_api_v1_test;

drop database react_on_rails_api_v4_development;
drop database react_on_rails_api_v4_test;




```


#########
# Example
#
```
rails g scaffold User email:string user:string password:string is_admin:boolean
rails db:migrate


```

#######################
# generate 'controller'
rails g controller welcome index 

############
# ['navbar' to test bootstrap](https://getbootstrap.com/docs/5.3/components/navbar/)

##############
# start server
./bin/dev

# ... also

rails g resource UserStock user:references stock:references

```

rails _6.1.7_ new [PROJECT_NAME] --css tailwind --database=postgresql

## list gems installed for current project
bundle list

## generate 'controller'
rails g controller welcome index 


######## CUSTOM.CSS ################
```
# assets/stylesheets/custom.css.scss
#
# if error 'cannot load file of type - sassc'
# add to Gemfile
# The original asset pipeline for Rails [https://github.com/rails/sprockets-rails]
gem 'sass-rails', '>= 5'
gem "sprockets-rails"

```

#############
## fontAwesome
```
bundle add font-awesome-rails

vi app/assets/stylesheets/custom.css.scss
--
@import 'font-awesome';
--
```

# RSpec
```
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

# list gems installed for current project
bundle list

```

## rails
```
###########
# STOCK API
#
rails g model Stock ticker:string name:string last_price:decimal
rails db:migrate

rails g controller users my_portfolio 
```

## bootstrap
rails new deploy_test_0 -b bootstrap --database=postgresql

## tailwind & postgres
rails new rails_alpha_blog --css tailwind --database=postgresql
cd rails_alpha_blog
rails db:prepare
rails tailwindcss:build 
rails s

## routes
rails routes --expanded

rails routes | grep <string i.e. model>

## controller
```
rails g controller pages_controller

vi ./config/routes.rb
--
  # Defines the root path route ("/")
  root "pages#home"

  get "/about", controller: "pages", action: :about

--

# views
<h1 class="text-3xl font-bold underline">
  Hello world!
</h1>

# migration
rails g migration create_articles 
vi create_articles
--
    create_table :articles do |t|
      t.string :title
      t.text :description
      t.timestamps
    end

--
rails db:migrate

```

## console - troubleshoot
```
rails c

article.save    # fails i.e does not satisfy validations
article.errors  # errors object
article.errors.full_messages  

```

## debugger
```
vi *_controller.rb
--
debugger

--
params    #list all received params
params[:id]
params[:controller]
params[:action]

continue
```

## Create Read Update Delete
### create
```
rails c

Article.all

# create new entry - method #1
Article.create(title: "first title", description: "first description")

# create new entry - method #2
article = Article.new
article.title = "second title"
article.description = "second description"
article.save

# create new entry - method #3
article = Article.new(title: "third title", description: "third description")
article.save

--

```
### read | update
```
Article.find(2)

Article.first
Article.last

article = Article.find(2)
article
article.title

articles = Article.all
```

### update
``` 
article.description = "updated second description"
Article.find(2)
article.save
Article.find(2)
```

### delete
```
article = Article.find(2)
article.destroy

Article.find(4).destroy

```


## git
```
git status
git add -A
git status
git commit -m "made changes"
```

## tailwind
- ### [Install Tailwind CSS with Ruby on Rails](https://tailwindcss.com/docs/installation/framework-guides/ruby-on-rails)
- ### [resources](https://v3.tailwindcss.com/resources)
  - ### [hero icons](https://heroicons.com/)

- ### [Use Tailwind CSS with Your Rails Forms](https://dev.to/railsdesigner/use-tailwind-css-with-your-rails-forms-4g1b)


# Object Oriented Programming - oop
**Encapsulation** - concept of blocking off areas of code and not making it available to the rest of the program

**Abstraction** - is simplifying a complex process of a program, an enterprise software solution for example by modeling classes appropriate for it

**Inheritance** - is used where a class inherits the behavior of another class, referred to as the superclass

**Polymorphism** - is when a class inherits the behaviors of another class, but has the ability to not inherit everything and change some of it’s inherited behaviors. For example to write a method that does something differently from the inherited method

**Classes** - It is a blueprint that describes the state and behavior that the objects of the class all share. A class can be used to create many objects. Objects created at runtime from a class are called instances of that particular class.

## [bcrypt-ruby](https://www.rubydoc.info/gems/bcrypt-ruby#:~:text=bcrypt%2Druby%20automatically%20handles%20the,letters%2C%20that's%20456%2C976%20different%20databases.)
```
gem install bcrypt
gem update --system 3.6.3

```

## Ruby Language Ref
```
# interactice ruby (irb) : ruby shell
irb

# triple equals '==='
# if the value on the right is a member of whatever is on the left

(1..10) === 1   # true
(1..10).include?(1)   # true

SUPPORTS_PATTERN_MATCH_VERSIONS = '2.7.0'..'3.0.0'
SUPPORTS_PATTERN_MATCH_VERSIONS === '2.7.5'   # true

/abc/ === 'abcdef'
# => true

/abc/.match?('abcdef')
# => true

/abc/ === 'abcdef'
# => true

/abc/.match?('abcdef')
# => true

# lambda

add_one_lambda = -> x { x + 1 }

add_one_lambda === 1
# => 2
add_one_lambda.call(1)
# => 2
add_one_lambda.(1)
# => 2
add_one_lambda[1]
# => 2
```

# AWS File Upload
```
# gem
bundle add carrierwave
bundle add mini_magick
bundle add fog
bundle add twitter-bootstrap-rails

rails g scaffold Image name:string picture:string user:references
rails db:migrate

#rails g boostrap:themed Images   # generatoe does not exist?

vi app/models/users.rb
--
  has_many :images
--

vi app/models/images.rb
--
  belongs_to :user
  mount_uploader :picture, PictureUploader

--

rails g uploader Picture


```


###################################
# FILE/IMAGE Uploads
#
```
rails g migration AddBioToUsers bio:text 
--
    add_column :users, :bio, :text
--

rails g migration AddContentToUsers content:text
--
    add_column :users, :content, :text
--

rails db:migrate

#######
# VIEWS
#
# app/views/devise/registrations (new & edit)
vi app/views/devise/registrations/new.html.erb
vi app/views/devise/registrations/edit.html.erb
--
      <div class="form-group col-md-6 mt-3">
        <%= f.label :bio %>
        <%= f.text_field :bio, autofocus: true, autocomplete: 'bio...', class: 'form-control' %>
      </div>
      
--

############
# CONTROLLER
#
vi app/controllers/applications_controller.rb
--
  before_action :authenticate_user!
  before_action :configure_permitted_parameters, if: :devise_controller?

  def stripe_payment; end

  # Only allow modern browsers supporting webp images, web push, badges, import maps, CSS nesting, and CSS :has.
  allow_browser versions: :modern

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [ :first_name, :last_name, :bio ])
    devise_parameter_sanitizer.permit(:account_update, keys: [ :first_name, :last_name, :bio ])
  end

--

##########
# GRAVATAR
#
vi app/helpers/application_helper.rb
--
require "digest"
require "uri"

module ApplicationHelper

  def gravatar_for(user, options={ size: 200 })
    # Assume you manually set the email_address here or get it from user input
    email_address = user.email.downcase
    
    # Create the SHA256 hash
    hash = Digest::SHA256.hexdigest(email_address)
    
    # Set default URL and size parameterss
    size = options[:size]
    default = "https://www.gravatar.com/avatar/?d=identicon&s=#{size}"

    # Compile the full URL with URI encoding for the parameters
    params = URI.encode_www_form('d' => default, 's' => size)
    image_src = "https://www.gravatar.com/avatar/#{hash}?#{params}"
    
    image_tag(image_src, alt: user.username, class: "rounded shadow mx-auto d-block")
  end
end

--

#######
# SHOW bio & gravatar
#
vi app/views/users/show.html.erb
--
<div class="card card-header results-block mt-3">
  <%= gravatar_for @user, size: 300 %>

  <div class="my-5">
    <h4 class="text-center text-white">BIO HERE<%= simple_format(@user.bio) %></h4>
  </div>
  
--

###########
# EDIT bio
#
vi app/views/devise/registrations/edit.html.erb
--

      <div class="form-group row">
        <%= f.label :bio, class: "col-2 mb-3 col-form-label text-light" %>
        <div class="col-10">
          <%= f.text_field :bio, class:"form-control mb-3 shadow rounded", placeholder: "your bio..." %>
        </div>
      </div>

--

jsnow@test.com
adfgadfgdfgdfgdfg

###########################
# EDIT - redirect on update
#
# Example subclass/override (registrations_controller.rb)
vi app/controllers/registrations_controller.rb
--
#class Users::RegistrationsController < Devise::RegistrationsController

#  protected

#  def after_update_path_for(resource)
#    user_path(resource)
#  end
#end

--

vi config/routes.rb
--
# Example routing config (in routes.rb):
#devise_for :users, :controllers => { :registrations => :registrations }

--


######################
# PERMITTED parameters

vi app/controllers/applications_controller.rb
--
  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:first_name, :last_name, :bio ])
    devise_parameter_sanitizer.permit(:account_update, keys: [:first_name, :last_name, :bio ])
  end
--

```

###################################################
## Single Image Uploads (AVATAR) - USE THIS VERSION
#
```
########
# CONFIG
#

######################
# MODEL - organization
#
vi app/models/user.rb
--
    has_one_attached :organization 

--


######################
# PERMITTED parameters

vi app/controllers/applications_controller.rb
--
  before_action :configure_permitted_parameters, if: :devise_controller?
  
  ...

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:first_name, :last_name, :bio, :avatar, :organization ])    # new
    devise_parameter_sanitizer.permit(:account_update, keys: [:first_name, :last_name, :bio, :avatar, :organization ])   # edit
  end
--

######################
# VIEWS - organization
#
vi app/devise/registrations/edit.html.erb   # EDIT
--
      <div class="form-group">
        <%= f.label :organization %>
        <%= f.file_field :organization, class: 'form-control' %>
      </div>
--

vi app/views/users/show.html.erb   # SHOW
--
  <%= @user.organization %>

--

vi app/devise/registrations/edit.html.erb   # NEW ?????????????????????????????
--


--

```


################################
## Single Image Uploads (AVATAR) - REMOVE!!
#
```
########
# CONFIG
#
rails active_storage:install

rails db:migrate

########
# ROUTES
#
vi config/routes.rb
--
  resources :users, except: [:new] do
    member do
      # remove_image_post_path(image)
      delete :remove_image
    end
  end
--

############
# MODEL
#
vi app/models/user.rb
--
  has_many_attached :images

--

############
# CONTROLLER
#
vi app/controllers/users_controller.rb
--
  def remove_image
    @image = ActiveStorage::Attachment.find(params[:id])
    @image.purge_later
    redirect_back(fallback_location: request.referer)
  end

  private
  ... 
  
  # Only allow a list of trusted parameters through.
  def user_params
    params.require(:post).permit(:username, :avatar, images: [])
  end
--


######################
# PERMITTED parameters
#
vi app/controllers/applications_controller.rb
--
  before_action :configure_permitted_parameters, if: :devise_controller?
  
  ...

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:first_name, :last_name, :bio, :avatar, images: [] ])    # new
    devise_parameter_sanitizer.permit(:account_update, keys: [:first_name, :last_name, :bio, :avatar, images: []])   # edit
  end
--



############
# VIEWS
#
vi app/devise/registrations/edit.html.erb   # EDIT
--
  <div>
    <%= f.label :images, "Upload images" %><br /><br />
    <%= f.file_field :images, multiple: true %>
  </div>
--

vi app/views/users/show.html.erb   # SHOW
--
  <% if user.images.attached? %>
    <% user.images.each do |image| %>
      <%= image_tag(image, height: 100) %>
      <%= link_to "Remove", remove_image_post_path(image), data: { turbo_method: :delete } %>
      <br />
    <% end %>
  <% end %>

--


#



```





#############
# Action Text
#
```
#########
# INSTALL
#
rails action_text:install

rails assets:precompile


#######
# MODEL
#
vi app/models/user.rb
--
  has_rich_text :content # for rich text editor

--

#######
# VIEWS
#

vi app/assets/config/manifest.js
--
...
//= link actiontext.css
--


vi app/views/layouts/application.html.erb
--
<%= stylesheet_link_tag 'actiontext' %> 

--


vi app/views/devise/registrations/edit.html.erb
--
      <div>
        <%= f.label :content, class: "col-2 mb-3 col-form-label text-light" %>
        <%= f.rich_text_area :content %> 
      </div>
--

vi app/views/users/show.html.erb
--
  <div class="my-5">
    <h4 class="text-center text-white"><%= @user.content %></h4>
  </div>

--

######################
# PERMITTED parameters

vi app/controllers/applications_controller.rb
--
  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [ :first_name, :last_name, :bio, :avatar, :content ])    # new
    devise_parameter_sanitizer.permit(:account_update, keys: [ :first_name, :last_name, :bio, :avatar, :content ])   # edit
  end
--

########
# DEPLOY
#
# ENSURE DB is completely RESET
#

vi config/application.rb
--
...
    initializer(:remove_action_mailbox_and_activestorage_routes, after: :add_routing_paths) { |app|
      app.routes_reloader.paths.delete_if {|path| path =~ /activestorage/}
      app.routes_reloader.paths.delete_if {|path| path =~ /actionmailbox/ }
    }
...
--

###############################
#### Google Cloud - DEPLOY ####
###############################
- ### see: gcloud-deploy-NOTES.md

# ENV
source ./config/.env-vars

###################
### cloudbuild.yaml
#
vi cloudbuild.yaml
--
  - id: "precompile action_text"
    name: "gcr.io/google-appengine/exec-wrapper"
    entrypoint: "bash"
    args:
      [
        "-c",
        "/buildstep/execute.sh -i gcr.io/${PROJECT_ID}/${_SERVICE_NAME} -s ${PROJECT_ID}:${_REGION}:${_INSTANCE_NAME} -e RAILS_MASTER_KEY=$$RAILS_KEY -- bundle exec rails assets:precompile"
      ]
    secretEnv: ["RAILS_KEY"]
--

######
BUCKET
#
h-pfolio-2-rails-fin-trac-v2-bucket-0

h-pfolio-2-rails-photo-app-v1-bucket-0

h-pfolio-2-image-upload-v2-bucket-0


##############
# SECRETS FILE
#
mkdir config/secrets

--

#####################
# SERVICE ACCOUNT KEY
#
“APIs and Services” => “Credentials” 
--
669532112637-compute@developer.gserviceaccount.com
--

"Manage Service Accounts"->"669532112637-compute@developer.gserviceaccount.com"->Triple-Dot->"Manage Keys"->"Add Key"->"Create New Key"->"JSON
--
copy downloaded key from Downloads to config/secrets/
rename file to 'gcs.keyfile'
ie

mv h-pfolio-2-7a3b4cc4fc5a.json gcs.keyfile

--

###########
# GITIGNORE
#
vi .gitignore
--
/config/secrets/*

--


################
# STORAGE Config
#
vi config/storage.yml
--
# Remember not to checkin your GCS keyfile to a repository
google:
  service: GCS
  project: rails_fin_track_7
  credentials: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_keyfile) %>
  bucket: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_bucket) %>

#  bucket: your_own_bucket-<%= Rails.env %>

google_dev:
  service: GCS
  project: rails_fin_track_7
  credentials: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_keyfile) %>
  bucket: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_bucket) %>

--

#############
# CREDENTIALS
#
EDITOR='subl --wait' ./bin/rails credentials:edit
--
gcp:
  db_password: trJfdEJtVlclBlHCqFWzuBQZOdtfOQbQExMkPtxmZgFCYSNjOk
  db_database: rails-photo-app-v2-db-0
  db_username: rails-photo-app-v2-user-0
  db_instance_host: h-pfolio-2:europe-west1:rails-photo-app-v2-instance-0
gcs_storage:
  gcs_bucket: h-pfolio-2-rails-photo-app-v2-bucket-0
  gcs_keyfile: config/secrets/gcs.keyfile
yahoo_finance_api:
  api_key: e7f6e7637fmshab60958cd514432p13a37fjsn26bb0feacaee
finnhub_api:
  api_key: cvamkq1r01qsapma7mj0cvamkq1r01qsapma7mjg
stripe:
  test_publishable_key: pk_test_51PLjRQGNOeNKBTc3wA60vBCW1bdhLVc4Y8wP4NX5E1cXiAh2TNYYgGThPvkcEYakAoOS6VNjFlKEBeweZsRnfQ1E009yKKEh2B
  test_secret_key: sk_test_51PLjRQGNOeNKBTc3eZKbQPlaUWyanXsmwNlHyHkiCUL0AmaG69zx6h2TOOxvXHm6XphlGbQTgr2ML9gzyEmj9mS600VMH0777F
  webhook_secret: whsec_6856dc192142555126bb021b6a9e4ea56bca247adb76a71438d4bd6d4f55c6c4
sendgrid_mailer:
  user_name: apikey
  api_key_secret: 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
  mail_sender: support@naught4profit.co.uk
  domain: naught4profit.co.uk

--
##############
# ENVIRONMENTS
#
vi config/environments/development.rb
--
config.active_storage.service = :google_dev

#####
# GEM
#
bundle add google-cloud-storage

```

############
## photo app
#
### upload photos (ARTIFACT)
```
##########
# ARTIFACT
#
rails generate scaffold Artifact name:string picture:string user:references
rails db:migrate

####################
# CARRIERWAVE CONFIG
#
vi config/initializers/carrier_wave.rb
--
    require 'carrierwave/storage/fog'
#    CarrierWave.configure do |config|
#      config.fog_provider = 'fog/aws'             # Use Fog for AWS compatibility
#      config.fog_credentials = {
#        provider:              'AWS',
#        aws_access_key_id:     ENV['AWS_ACCESS_KEY_ID'],       # Your AWS access key
#        aws_secret_access_key: ENV['AWS_SECRET_ACCESS_KEY'],   # Your AWS secret access key
#        region:                ENV['AWS_REGION'],              # The region of your Spaces bucket
#        endpoint:             ENV['AWS_ENDPOINT'] # The endpoint URL of your Spaces bucket
#      }
#      config.fog_directory  =  ENV['AWS_BUCKET']  # The name of your Spaces bucket 
#      config.fog_public     = true               # Set to true if you want to make uploaded files publicly accessible 
#      config.storage        = :fog               # Use fog storage 
#    end
--

```

##########
# GEMS
#

bundle add image_processing
bundle add carrierwave
bundle add streamio-ffmpeg
bundle add mini_magick
bundle add fog
bundle add ffi
bundle add google-cloud-storage

#gem "fog-aws"
#gem 'dotenv-rails', '2.7.6'

##########
# UPLOADER
#
rails generate uploader Artifact

vi app/uploaders/artifact_uploader.rb
--
  # uncomment the following
...
include CarrierWave::MiniMagick
...
version :thumb do
  process :resize_to_fill => [50, 50]
end
...
def extension_allowlist
  %w(jpg jpeg gif png)
end
...

###############
# MODEL - Image
#
vi app/model/artifact.rb
--
mount_uploader :artifact, ArtifactUploader

--

#######################
# Validating Image Size
#
vi app/model/artifact.rb
--
validates_processing_of :artifact
validate :image_size_validation

private

def image_size_validation
  errors[:image] << "should be less than 500KB" if image.size > 0.5.megabytes
end

--




#########################################
## Single Image Uploads (ARTIFACT.ARTIMG)
#
```
######
# PREP
#
rails g migration AddArtimgToArtifact artimg:string
--
    add_column :artifact, :artimg, :string
--

rails db:migrate

# enable 'MiniMagick'
vi app/config/environments/development.rb
``

  # Store uploaded files on the local file system (see config/storage.yml for options).
  config.active_storage.service = :local
  config.active_storage.variant_processor = :mini_magick
``

########
# CONFIG
#
rails active_storage:install

rails db:migrate

########
# ROUTES
#
vi config/routes.rb
--
  resources :artifact, except: [:new] do
    member do
      # remove_artimg_user_path(artimg)
      delete :remove_artimg
    end
  end
--

############
# MODEL
#
vi app/models/artifact.rb
--
  has_one_attached :artimg  #for single image upload


--

############
# CONTROLLER
#
vi app/controllers/artifacts_controller.rb
--
  def remove_artimg
    @artimg = ActiveStorage::Attachment.find(params[:id])
    @artimg.purge_later
    redirect_back(fallback_location: request.referer)
  end

  private
  ... 
    # Only allow a list of trusted parameters through.
    def artifact_params
      params.require(:artifact).permit(:name, :picture, :user_id, :artimg)
    end
--


######################
# PERMITTED parameters - if modifyinh DEVISE login fields
#
vi app/controllers/applications_controller.rb
--
  before_action :configure_permitted_parameters, if: :devise_controller?
  
  ...

  protected

  def configure_permitted_parameters
    devise_parameter_sanitizer.permit(:sign_up, keys: [:first_name, :last_name, :bio, :avatar, images: [] ])    # new
    devise_parameter_sanitizer.permit(:account_update, keys: [:first_name, :last_name, :bio, :avatar, images: []])   # edit
  end
--

############
# VIEWS
#
# fontawesomeL
# https://fontawesome.com/v4/icons/
#
vi app/views/artifacts/_form.html.erb   # EDIT
--
  <div>
    <%= f.label :artimg, "Attach image" %><br /><br />
    <%= f.file_field :artimg %>
  </div>
--

vi app/views/artifact/show.html.erb   # SHOW
--
      <%= image_tag(artifact.artimg, height: 100) %>
      <%= link_to "Remove", remove_image_post_path(artimg), data: { turbo_method: :delete } %>



  <% if artifact.artimg.attached? %>
    <% artifact.artimg.each do |arti| %>

      <%= image_tag(arti, height: 100) %>
      <%= link_to "Remove", remove_image_post_path(artimg), data: { turbo_method: :delete } %>
      <br />

    <% end %>
  <% end %>

--

```

#############
# VALIDATIONS
- ### [Active Record Validations and Callbacks](https://guides.rubyonrails.org/v3.2/active_record_validations_callbacks.html)
#
```

vi app/models/artifact.rb
--
        validates :name, presence: true
        validates :artimg, presence: true

        has_one_attached :artimg
        validate :artimg_size_limit

        private

        def artimg_size_limit
          return unless artimg.attached?
          if artimg.blob.byte_size > 1.megabytes
            errors.add(:artimg, "must be less than 1MB")
          end
        end

--

```

#################################
# RESIZE_TO_LIMIT : max file size
#
```
vi app/models/artifact.rb
--
class Artifact < ApplicationRecord
  has_one_attached :artimg do |attachable|
    attachable.variant :thumb, resize_to_limit: [238, 238]
  end
end
--

vi app/views/artifact/_form.html.erb
--
    <%= image_tag artifact.artimg.variant(:thumb) %>
--

```

#####################
# PHOTO APP - SEED Db
- ### [Seed data with image attachments via ActiveStorage](https://medium.com/@andrewasmit/seed-data-with-image-attachments-via-activestorage-45c8516c19ea)

#
```
```


##############
# MULT-TENANCY : add to alpha-blog
- ### [rails_alpha_blog_with_ActAsTenant_v1](https://github.com/heidless-stillwater/rails_alpha_blog_with_ActAsTenant_v1)
#
```


```


##########################
# Organization LOGO upload
#

rails g migration AddLogoToOrganizations logo:string
rails db:migrate


############
# CONTROLLER
#
vi app/controllers/organizations_controller.rb
--
  # Only allow a list of trusted parameters through.
  def organization_params
    params.require(:organization).permit(:domain, :subdomain, :active_org, :name, :user_id, :logo)
  end

--

#######
# MODEL
#
vi app/models/organizations/organization.rb
--
  has_one_attached :logo  #for single image upload

--

#######
# VIEWS
#
vi app/views/organizations/_organization.html.erb
--
      <div class="img-responsive center-block">      
        <% if current_user.logo.present? %>  
          <%= image_tag @organization.logo, width: "200", class: "mx-auto d-block" %>
        <% else %>
          <h1 class="text-center my-5"> No Logo </h1>
        <% end %>
      </div>
--

vi app/views/organizations/_form.html.erb
--
...
      <div class="form-row">
        <div class="form-group col-md-6 mt-3">
          <%= f.label :logo %>
          <%= f.file_field :logo, class: 'form-control' %>
        </div>
      </div>
...

--

#############################################
#############################################

###########################################
# PLAN Scaffold
#
```

rails generate scaffold Plan name:string amount:decimal symbol:string organization:references

rails g migration SetDefaultOrgForPlan
--
  change_column :plans, :organization_id, :bigint, default: 1
--

############
# CONTROLLER
#
vi app/plans/plans_controller.rb
--
  # Only allow a list of trusted parameters through.
  def plan_params
    params.require(:organization).permit(:name, :amount, :symbol, :organization_id)
  end

--

#######
# MODEL
#
vi app/models/plan.rb
--
  has_one_attached :symbol  #for single image upload

--

#######
# VIEWS
#
vi app/views/plans/_plan.html.erb
--
...
  as for app/views/organization/_organization.html.erb
...
--

vi app/views/plans/_form.html.erb
--
...
  as for app/views/organization/_organization.html.erb

...

--


##########################
# Organization LOGO upload
#

rails g migration AddImageToProjects image:string
rails db:migrate


############
# CONTROLLER
#
vi app/controllers/projects_controller.rb
--
  # Only allow a list of trusted parameters through.
    def project_params
      params.require(:project).permit(:name, :details, :expected_completion_date, :organization_id, :image)
    end

--

#######
# MODEL
#
vi app/models/projects/project.rb
--
  has_one_attached :image  #for single image upload

--

#######
# VIEWS
#
vi app/views/projects/_project.html.erb
--
      <div class="img-responsive center-block">      
        <% if current_user.logo.present? %>  
          <%= image_tag @organization.logo, width: "200", class: "mx-auto d-block" %>
        <% else %>
          <h1 class="text-center my-5"> No Logo </h1>
        <% end %>
      </div>
--

vi app/views/organizations/_form.html.erb
--
...
      <div class="form-row">
        <div class="form-group col-md-6 mt-3">
          <%= f.label :logo %>
          <%= f.file_field :logo, class: 'form-control' %>
        </div>
      </div>
...

--