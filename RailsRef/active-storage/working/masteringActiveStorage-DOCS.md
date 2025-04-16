
## [Upload multiple images To Rails 7 API - Active_storage : Part 1](https://dev.to/chrissiku/upload-multiple-images-to-rails-7-api-activestorage-2o6p)

## [Mastering Active Storage: Powering Professional Image Uploads in Rails with Active Storage and Action Text](https://medium.com/@adi8090808766/mastering-active-storage-powering-professional-image-uploads-in-rails-with-active-storage-and-aedb93e3a8ea)

## SETUP BASE APP

# GIT Home for this app
```
# image_upload_v2
git@github.com:heidless-stillwater/image-upload-v2.git

```

### add BIO
```
rails g migration AddBioToUsers bio:text 
rails db:migrate

rails g migration AddAvatarToUsers avatar:string
rails db:migrate

rails g migration AddContentToUsers content:text
rails db:migrate


############
# CONTROLLER
#
vi app/controllers/users_controller.rb
--
  def user_params
    params.require(:user).permit(:username, :email, :password, :admin, :bio, :avatar, :content)

  end

--

######
# SHOW
#
vi app/views/articlae/show.html.erb
--

  <%= gravatar_for @user, size: 300 %>

  <div class="my-5">
    <h4 class="text-center text-white"><%= simple_format(@user.bio) %></h4>
  </div>

--

######
# EDIT
#
vi app/views/articlae/_form_.html.erb
--
      <div class="form-group row">
        <%= f.label :bio, class: "col-2 mb-3 col-form-label text-light" %>
        <div class="col-10">
          <%= f.text_field :bio, class:"form-control mb-3 shadow rounded", placeholder: "your bio..." %>
        </div>
      </div>

```

### Active Storage & Active Text
```
rails active_storage:install

rails action_text:install
--
'Y' to over-write
--

# Ensure gem present
gem "image_processing"

rails db:migrate

# enale 'MiniMagick'
vi app/config/environments/development.rb
``
config.active_storage.variant_processor = :mini_magick;

``

```

### model (for 'avatar')
```
vi app\models\user.rb
--
class User < ApplicationRecord
    has_one_attached :avatar  #for single image upload
    # has_many_attached :images #for multiple image upload
    # has_rich_text :content #for rich text editor
end
--
```

### CONTROLLER (for 'avatar')
```
vi app\controllers\users_controller.rb
--
def user_params
  # params.require(:user).permit(:name, :bio, :avatar, images: [], :content)
  params.require(:user).permit(:username, :email, :password, :admin, :bio, :avatar)
end
```

### FORM (for ALL)
for rich_text, avatar & images
delete whichever are not in use
```
vi app\views\users\_form_.html.erb
--
#app\views\users\_form.html.erb
#add following form line for relevent from control on user or your model

<!--
  <div>
    <%= form.rich_text_area :content %>  <%# This is the rich text editor %>
  </div>
-->

  <div>
    <%= form.label :avatar %>
    <%= form.file_field :avatar %> <%# This is the avatar. i.e. single image uplaod %>
  </div>

<!--
  <div>
    <%= form.label :images %>
    <%= form.file_field :images, multiple: true %> <%# This is the images. i.e. multiple image upload %>
  </div>
-->
--

```

### VIEW (for ALL)
Delete those not used
```
vi app\views\users\_user.html.erb
--
  #add following form line for relevent view on user 

  <%= user.content %> <br /> <%# this is for viewing the reach content attachment we add i.e rich content %>
  
  <%= image_tag user.avatar %> <br /> <%# this is for viewing the avatar attachment we add i.e  single image%>

  <% user.images.each do |image| %> <%# this is for viewing the images attachment we add i.e multiple image %>
    <%= image_tag image %>
  <% end %>
--

  ```