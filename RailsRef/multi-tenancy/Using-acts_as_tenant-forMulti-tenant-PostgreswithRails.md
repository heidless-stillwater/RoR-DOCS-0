
## [Using acts_as_tenant for Multi-tenant Postgres with Rails](https://www.crunchydata.com/blog/using-acts_as_tenant-for-multi-tenant-postgres-with-rails)

### acts_as_tenant
acts_as_tenant has recently released version 1.0 after 12 years of development — so it’s not new. 

The gem implements multi-tenant best-practices by augmenting Rails’ ActiveRecord ORM:
- protects developers from building queries that return other tenant’s records
- requires a tenant_id on the tables for models specific to a tenant
- adds the tenant_id scope to the query
- includes ActionController, ActiveRecord, ActiveJob helpers to insert new records with the scoped tenant

**Acts_as_tenant** is built for **row-level** multi-tenancy, and that is it. 

So, no need to manage multiple databases or schemas for data structures — it keeps it simple. 

One of the best things I can say about acts_as_tenant is that it can be implemented by an existing application code-base. 

Too many times, with the older multi-tenant gems, the implementation was invasive, and thus required complex refactoring.

**What it’s not**: acts_as_tenant is not for account-based sharding — either schema-based or multi-cluster based sharding. **It’s purely for multi-tenant safety**.

# Example App
## create
```
########
# CREATE
#
rails _7.2.2.1_ new acts_as_tenent_app_v2 -j esbuild --css bootstrap --database=postgresql

cd acts_as_tenent_app_v2

rails db:prepare

rails g model Account name:string
rails g model User email:string account_id:integer
rails g model Post content:string user_id:integer account_id:integer

rails db:migrate

########
# CONFIG
#

bundle add acts_as_tenant

vi app/models/account.rb
--
  has_many :users
  has_many :posts

--

vi app/models/post.rb
--
  belongs_to :user
  acts_as_tenant :account

--

vi app/models/user.rb
--
  acts_as_tenant :account
  validates_uniqueness_to_tenant :email

--

```

## experiment
```
rails c

first_account = Account.create!(name: "First Account")
last_account = Account.create!(name: "Last Account")

ActsAsTenant.with_tenant(first_account) do
  user = User.create!(email: "test@example.com")
  post = Post.create!(user: user, content: "Lorem Ipsum")
end

ActsAsTenant.with_tenant(first_account) do
  Post.first.content # -> "Lorem Ipsum"
end

ActsAsTenant.with_tenant(last_account) do
  Post.first.nil? # -> true because we did not create a tenant
end

Post.first.content # -> "Lorem Ipsum"

ActsAsTenant.configure do |config|
  config.require_tenant = true
end

Post.first.content # -> ActsAsTenant::Errors::NoTenantSet (ActsAsTenant::Errors::NoTenantSet)

```

## application_controller
```

vi app/controllers/application_controller.rb
--
class ApplicationController < ActionController::Base
  set_current_tenant_through_filter
  before_action :require_authentication
  before_action :set_tenant

  def require_authentication
    current_user || redirect_to(new_session_path)
  end

  def current_user
    @current_user ||= if session[:user_id].present?
			User.find(session[:user_id])
    end
  end

  def current_acount
    @current_account ||= current_user.try(:account)
  end

  def set_tenant
    set_current_tenant(current_account)
  end
end

--

```



