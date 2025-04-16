

# [GIT Src](git@github.com:Deanout/multiple_api_active_storage.gsit)

```

git clone git@github.com:Deanout/multiple_api_active_storage.git

cd multiple_api_active_storage

rvm install 3.2.0

bundle install 

rails db:prepare

rails s

```


# [Upload Multiple Images To Active Storage API! | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=U1IwoFo5pJA)
```
rails _7.2.2.1_ new video_api  --api --database=postgresql
cd video_api

rails g scaffold post

rails active_storage:install

#####
# GEM
#
vi Gemfile
--
# Use Active Storage variants [https://guides.rubyonrails.org/active_storage_overview.html#transforming-images]
gem "image_processing", "~> 1.2"

# Use Rack CORS for handling Cross-Origin Resource Sharing (CORS), making cross-origin Ajax possible
gem "rack-cors"

--
bundle install

######
# CORS
#
vi config/initializers/cors.rb
--
# uncomment all.
# convert 'origins "example.com"' to 'origins "*"'

########
# MODELS
#
vi app/models/post.rb
--
class Post < ApplicationRecord
  has_many_attached :images
end
--

vi app/controllers/posts_controller.rb
--
def post_params
  params.require(:post).permit(images: [])
end


--

rails db:prepare

rails s


```

# 

