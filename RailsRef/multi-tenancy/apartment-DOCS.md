

#######################################
# Multi-Tenancy - APARTMENT
- ### [Set up a basic multi-tenant architecture in Rails 7 - 1](https://dev.to/harsh_u115/set-up-a-basic-multi-tenant-architecture-in-rails-7-4445)
```
#####
# GEM
#
bundle add apartment

##################
# MODEL - Hospital
#
rails g model Hospital name

# Add the tenant identifier column to each table:
rails g migration AddHospitalIdToPatients hospital_id:integer

############################
# INITIALIZER = apartment.rb
#
vi app/config/apartment.rb
--
Apartment.configure do |config|
  config.excluded_models = %w[Hospital]
  config.tenant_names = lambda { Hospital.pluck(:name) }
end

--

```

###############
# TENANT Switch 
# Use the tenant identifier to switch between schemas in your controllers:
  - ### [apartment](https://github.com/rails-on-services/apartment)
```
vi app/contollers/application_controller.rb
--
class ApplicationController < ActionController::Base
  before_action :set_tenant

  private

  def set_tenant
    hospital = Hospital.find_by(name: request.subdomain)
    Apartment::Tenant.switch(hospital.name) if hospital
  end
end

--

##############
# SIGNUP/LOGIN
#
vi app/controllers/hospital_controller.rb
--
class HospitalsController < ApplicationController
  def new
    @hospital = Hospital.new
  end

  def create
    hospital = Hospital.create!(hospital_params)
    Apartment::Tenant.create(hospital.name)
    redirect_to root_url(subdomain: hospital.name)
  end

  private

  def hospital_params
    params.require(:hospital).permit(:name)
  end
end

--

################
# PATIENTS Model
#
--
class CreatePatients < ActiveRecord::Migration[6.0]
  def change
    create_table :patients do |t|
      t.string :first_name
      t.string :last_name
      t.string :email
      t.integer :hospital_id
      t.timestamps
    end
  end
end
--

##############
# APPOINTMENTS Model
#
--
class CreateAppointments < ActiveRecord::Migration[6.0]
  def change
    create_table :appointments do |t|
      t.datetime :date_time
      t.integer :patient_id
      t.integer :hospital_id
      t.timestamps
    end
  end
end

--

```