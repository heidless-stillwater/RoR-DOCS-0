
## [Active Storage Overview](https://guides.rubyonrails.org/active_storage_overview.html)

## [Codecast: Getting Started with Rails 7 24: Use ActionText for Rich Text Content](https://www.youtube.com/watch?v=o5vFR1u4qMw)
- ### [PROJECT: image_upload_v2](git@github.com:heidless-stillwater/image_upload_v2.git)
```

################
# PRE=REQUISITES
#
sudo apt install -f libvips-tools libvips-dev libvips42

bundle add ruby-vips
bundle add mini_magick
bundle add ffi
bundle add image_processing

rails action_text:install

rails assets:precompile

bundle install

rails db:migrate

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



```