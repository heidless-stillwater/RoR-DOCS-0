


## RAILS: Upload multiple images To Rails 7 API
  - ## [Part 1: API](https://dev.to/chrissiku/upload-multiple-images-to-rails-7-api-activestorage-2o6p)
    -  ### GITHUB: [multiple-image-uploads-api](git@github.com:Chrissiku/multiple-image-uploads-api.git)
  - ## [Part 2: Frontend](https://dev.to/chrissiku/upload-multiple-images-to-rails-7-api-activestorage-part-2-4dll)
    - ### GITHUB: [multiple-image-uploads-client](git@github.com:Chrissiku/multiple-image-uploads-client.git)

# API
```
rails new heidless_multi_image_api_v1 --api -d postgresql 

cd heidless_multi_image_api

bundle install

rails db:prepare

#######
# MODEL
#
rails g scaffold post

################
# ACTIVE STORAGE
#
rails active_storage:install

#########
# GEMFILE
#
bundle add image_processing
bundle add rack-cors

########################################
# To allow all origins to access the API
#
vi config/initializers/cors.rb
--
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins "*"

    resource "*",
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end

--

#######
# MODEL
#
vi app/models/post.rb
--
class Post < ApplicationRecord
  has_many_attached :images
end

--

############
# CONTROLLER
# -> modify to handle image uploads
#
vi app/controllers/posts_controller.rb
--
class PostsController < ApplicationController
  before_action :set_post, only: %i[show update destroy]

  # GET /posts
  def index
    @posts = Post.all

    render json: @posts
  end

  # GET /posts/1
  def show
    render json: @post.as_json(include: :images).merge(
      images: @post.images.map do |image|
        url_for(image)
      end,
    )
  end

  def create
    # The `Post` model should be defined or required in this file.
    @post = Post.new(post_params.except(:images))

    # The `save` method should be called after images have been attached.
    if @post.save
      images = params[:post][:images]

      if images
        images.each do |image|
          @post.images.attach(image)
        end
      end

      render json: @post, status: :created, location: @post
    else
      render json: @post.errors, status: :unprocessable_entity
    end
  end

  # PATCH/PUT /posts/1
  def update
    if @post.update(post_params)
      render json: @post
    else
      render json: @post.errors, status: :unprocessable_entity
    end
  end

  # DELETE /posts/1
  def destroy
    @post.destroy
  end

  private

  # Use callbacks to share common setup or constraints between actions.
  def set_post
    @post = Post.find(params[:id])
  end

  # Only allow a list of trusted parameters through.
  def post_params
    params.require(:post).permit(images: [])
  end
end

```

# CLIENT
```
##########
# INIT
#
npm install
npm install -g npm@11.2.0

npm run dev

```




