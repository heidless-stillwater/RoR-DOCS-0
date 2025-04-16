
## [Add ActsAsTenant to an existing application](https://www.youtube.com/watch?v=YzvWRzNwdiM)
- ### [GIT:acts_as_tenant](https://github.com/ErwinM/acts_as_tenant)
- ### [EXEMPLAR: photo-app-ActsAsTenant-v1](https://github.com/heidless-stillwater/photo-app-ActsAsTenant-v1#)


########################
# ACTS_AS_TENANT - NOTES
- ### [ActsAsTenant Gem Walkthrough](https://www.youtube.com/watch?v=BIyxM9f8Jus)
```
bundle add acts_as_tenant


rails g migration AddDomainInfoToOrganization
--
  add_column :organizations, :domain, :string, null: true 
  add_column :organizations, :subdomain, :string, null: true 

--

user.rb, payment.rb, artifact.rb
--
acts_as_tenant :organization
...
--

application_controller.rb
--
set_current_tenant_by_subdomain_or_domain(:organization, :subdomain, :domain)

--

############
# INVOKE - using localhost emulator : lvh.me
photoapp.lvh.me:3000

application.rb
--

module PhotoAppV1
  class Application < Rails::Application

  ...

  config.hosts = nil

--
```






##########################
# RAILS CONSOLE CHEATSHEET
#
```
###
# Clean list of names. This is my favorite.
ActiveRecord::Base.descendants.map {|type| puts type}

###
# Raw list of tables
ActiveRecord::Base.connection.tables

###
# Clean list of names
Rails.application.eager_load!
ApplicationRecord.descendants.collect { |type| type.name }

###
# Detailed list of models with parameters.
ActiveRecord::Base.connection.tables.map{|x|x.classify.safe_constantize}.compact

```

# Project 
```
git clone git@github.com:heidless-stillwater/photo-app-ActsAsTenant-v1.git


#######
# USERS
--
jsnow
jsnow@test.com
password
--

```

###########################
# GIT, DB Snapshot & gCloud
#
```
##########
# GIT
#
git init

git checkout -b BASELINE-0

# BACKUP
cd BACKUPS
<take local DB backup>

# GOOGLE
<configure>

```

################
# ACTS_AS_TENANT
#

# INSTALL
```
#####
# GEM
#
bundle add acts_as_tenant


##########
# RESET DB
#
# restart POSTGRES
sudo systemctl restart postgresql

psql
--
DROP DATABASE photo_app_acts_as_tenant_v1_development;
DROP DATABASE photo_app_acts_as_tenant_v1_test;

--
rails db:prepare

--

############
# MIGRATIONS
#
rails generate model Organization user:references name:string
# allow for user_id = nil
--
class CreateOrganizations < ActiveRecord::Migration[7.2]
  def change
    create_table :organizations do |t|
      # t.references :user, null: false, foreign_key: true

      t.references :user, null: true, foreign_key: true
      t.string :name

      t.timestamps
    end
  end
end


--

rails generate migration add_organization_as_tenancy
--
class AddOrganizationTenancy < ActiveRecord::Migration[6.0]
  def change
    # users
    add_column :users, :organization_id, :integer
    add_index :users, :organization_id

    # payments
    add_column :payments, :organization_id, :integer
    add_index :payments, :organization_id

    # artifacts
    add_column :artifacts, :organization_id, :integer
    add_index :artifacts, :organization_id
  end
end
--
rails db:migrate


##################
# set ORGANIZATION
# manually set first organization
#
rails c
--
org = Organization.new(name: "Lumin Inc", user_id: nil)
org.save

--

############
# CONTROLLER
#
# SET TENANCY_ID: ACCOUNT_ID
#
# Setting current tenant in the application controller
#
vi app/controllers/application_controller.rb
--
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
  set_current_tenant_through_filter
  before_action :set_current_organization

  def set_current_organization
    return unless current_user.present?
    current_organization = current_user.organization
    ActsAsTenant.current_tenant = current_account
  end
end

--

########
# MODELS
#
vi app/models/user.rb
--
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable

  has_one :organization

  before_validation :set_organization

  def set_organization
    #self.build_account
    #self.build_organization
    self.organization = 1
  end
end

--

vi app/models/organization.rb
--
  belongs_to :user

--

```

########################
## CONFIG ACTS_AS_TENANT
#
```

vi app/models/user.rb
--
acts_as_tenant :organization
...
--

vi app/models/artifact.rb
--
acts_as_tenant :organization
...
--

vi app/models/payment.rb
--
acts_as_tenant :organization
...
--

```

######################################
# integrate ACTS_AS_TENANT with STRIPE
#
```

#######
# VIEWS
#
# app/views/devise/registrations (new & edit)
vi app/views/devise/registrations/new.html.erb
vi app/views/devise/registrations/edit.html.erb
--
      <div class="form-group col-md-6 mt-3">
        <%= f.label :organization %>
        <%= f.number_field :organization_id, autofocus: true, autocomplete: 'organization id...', class: 'form-control' %>
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

  protected

  def configure_permitted_parameter_fields
    devise_parameter_sanitizer.permit(:sign_up, keys: [ :organization_id, :avatar, :username, images: [] ])    # new
    devise_parameter_sanitizer.permit(:account_update, keys: [ :organization_id, :avatar, :username, images: [] ])   # edit
  end

--

```

###################
# ORGANIZATIONS MVC
#
```
# CONTROLLER
#
psql
--
DROP DATABASE photo_app_acts_as_tenant_v1_development;
DROP DATABASE photo_app_acts_as_tenant_v1_test;

--

rails generate scaffold Organization name:string user:references

rails g migration AddActiveOrgToOrganizations
--
  def change
    add_column :organizations, :active_org, :boolean, default: true, null: true
  end
--

rails g migration DropActiveOrgFromOrganizations
--
  remove_column(:organizations, :active_org, if_exists: true)

--


rails g migration RestoreActiveOrgToOrganizations
--
  def change
    add_column :organizations, :active_org, :boolean, default: false, null: true
  end
--

rails g migeation SetDefaultToFalseInOrganizations
--
    change_column_default :organizations, :active_org, false 

--

Organization.create!(id: 3, name: "Alphabet - NEW", active_org: false)

```

##################
# PLANS - add to Organization
#
```
rails g migration AddPlanToOrganizations plan:string

rails g migration AddPlanToUsers plan:string

########
# MODELS
#
vi app/models/organization.rb
--
...
  has_one :plan
...
--

vi app/models/user.rb
--
...
  has_one :plan
...
--


rails generate scaffold Plan name:string amount:decimal user:references


