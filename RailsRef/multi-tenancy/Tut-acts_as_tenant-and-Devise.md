
## [Multitenant Rails Application using acts_as_tenant and Devise](https://www.mintbit.com/blog/multitenant-rails-application-using-acts-as-tenant-and-devise/)


## Project
```
rails _7.2.2.1_ new act-as-tenent-app-v1 -j esbuild --css bootstrap --database=postgresql
rails db:prepare

```

## Devise
```
#####
# gem
bundle add devise

############
# generators
#
rails generate devise:install
rails generate devise User

```

# Welone Model
```
rails g controller Welcome index

vi config/routes.rb
--
  root "welcome#index"

--

```

## Scaffolding & Models
```
rails generate scaffold Project name

rails generate model Account user:references

rails generate migration add_account_to_projects
--
class AddAccountToProjects < ActiveRecord::Migration[6.0]
  def change
    add_column :projects, :account_id, :integer
    add_index :projects, :account_id
  end
end

--

rails db:migrate

```

## act_as_tenant
```
bundle add acts_as_tenant

vi app/models/user.rb
--
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable

  has_one :account

  before_validation :set_account

  def set_account
    self.build_account
  end
end

--

vi app/models/account.rb
--
  belongs_to :user

--


```

## Setting current tenant in the application controller
```
vi app/controllers/application_controller.rb
--
class ApplicationController < ActionController::Base
  before_action :authenticate_user!
  set_current_tenant_through_filter
  before_action :set_current_account

  def set_current_account
    return unless current_user.present?
    current_account = current_user.account
    ActsAsTenant.current_tenant = current_account
  end
end

--

```