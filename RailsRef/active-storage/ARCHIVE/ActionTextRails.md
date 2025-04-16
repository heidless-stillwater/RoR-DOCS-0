
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

bundle install

rails db:migrate

#######
# MODEL
#
vi app/models/user.rb
--
  has_rich_text :content #for rich text editor

--

#######
# VIEWS
#
vi app/views/users/_form.html

rails assets:precompile





```