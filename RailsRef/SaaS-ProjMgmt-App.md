
# [RoR-Developer-Course](https://www.udemy.com/course/the-complete-ruby-on-rails-developer-course/learn/lecture/4026084#notes)
- 
```

rails _7.2.2.1_ new saas-project-app-v1 -j esbuild --css bootstrap --database=postgresql
rails db:prepare

######
# GEMS
#
# bundle add ruby-vips  # issue with Google build
bundle add font-awesome-rails
bundle add image_processing
bundle add carrierwave
bundle add streamio-ffmpeg
bundle add mini_magick
bundle add fog
bundle add ffi
bundle add google-cloud-storage

######
# HOME
rails g controller home index

#########
# FAVICON
#
https://www.flaticon.com/free-icons/favicon

vi app/views/application.html.erb
--
    <link rel="icon" type="image/png" href="https://storage.googleapis.com/h-pfolio-2-saas-project-app-v1-bucket-0/images/favicon.png"/>
--


```

#########
# DEPLOY
#
```
EDITOR='subl --wait' ./bin/rails credentials:edit
--
=======================================
# saas-project-app-v1
#
gcp:
  db_password: DmtHMovGmeXLocTpOkNCxErggZIaBJupGHiuwDvFojCEtXZSNU
  db_database: saas-project-app-v1-db-0
  db_username: saas-project-app-v1-user-0
  db_instance_host: h-pfolio-2:europe-west1:saas-project-app-v1-instance-0
  app_url: https://saas-project-app-v1-svc-669532112637.europe-west1.run.app
gcs_storage:
  gcs_bucket: h-pfolio-2-saas-project-app-v1-bucket-0
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
  mail_sender: lockhart.r@gmail.com
  domain: naught4profit.co.uk

--

```

################
# SENGRID Config
#
- as in /home/heidless/projects/heidless-DOCS/RoR-DOCS-0/RailsRef/sendgrid/sendgrid-DOCS.md


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


#############################################
#############################################

###########################################
# PROJECT Scaffold
#
```

rails generate scaffold Project name:string details:string expected_completion_date:date organization:belongs_to

rails g migration AddPlanToUsers plan:references

rails g migration AddPlanToOrganizations plan:references

rails g migration AddUserToProjects user:references

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

UPDATE organizations SET plan_id = 2;

WHERE brand = 'Volvo';



#####################
# ITEMS scaffold
```
rails g scaffold Item name:string key:string project:belongs_to

#rails g migration AddOrganizationIdToItems organization:references
#rails g migration RemoveOrganizationFromItems organization:references

# app/models/project.rb
--
  has_many :items, dependent: :destroy # for multiple items
--

# app/models/item.rb
--
class Item < ApplicationRecord
  before_save :upload_item_to_gcs
  attr_accessor :upload # for file uploads
  belongs_to :project

  MAX_FILE_SIZE = 10.megabytes

  validates_presence_of :name, :upload
  validates_uniqueness_of :name

  validate :upload_file_size

  private

  def upload_item_to_gcs
    this_org = @org_active
    blob = artimg.blob
    current_object_key = blog.key
    new_object_key = "#{this_org}/#{current_object_key}" # Construct the new object key
    blob.upload(new_object_key)
  end

  def upload_file_size
    if upload.size > MAX_FILE_SIZE
      errors.add(:upload, "File size exceeds the maximum limit of #{MAX_FILE_SIZE / 1.megabyte} MB")
    end
  rescue NoMethodError
    # Handle the case where upload is nil or not an object with size method
    errors.add(:upload, "Invalid file upload")
  end
end
...
--

```



############################
### upload photos (ARTIFACT)
```
##########
# ITEMS
#
rails generate scaffold Item name:string description:string project:belongs_to --force
rails db:migrate

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

git reset --merge 3f5a269

#################################################################
########################## ITEM.item_img ########################
#################################################################
## Single Image Uploads (ITEM.item_img) - USE THIS VERSION
#
```
########
# CONFIG
#
rails g migration AddImageToItems item_img:string
rails db:migrate

###########
# CONTROLLER
#
vi app/controllers/item_controller.rb
--
    # Only allow a list of trusted parameters through.
    def item_params
      params.require(:item).permit(:name, :description, :project_id, :item_img)
    end
--


#######
# VIEWS
#
vi app/views/items/_form.html.erb   # EDIT
--
      <div class="form-group">
        <%= f.label :image %>
        <%= f.file_field :item_img, class: 'form-control' %>
      </div>
--

vi app/views/items/_item.html.erb   # SHOW
--
  <%= @item.item_img %>

--

```

#######
# ADMIN
#
```
rails g migration add_is_admin_to_users is_admin:boolean
--
class AddIsAdminToUsers < ActiveRecord::Migration[7.2]
  def change
    add_column :users, :is_admin, :boolean, default: false
  end
end
--

rails db:migrate

# app/models/user.rb
--

  def is_admin?
    is_admin
  end
--

# app/controllers/devise/registrations/

```