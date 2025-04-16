
# Candidate Tuts
- ### [Image Previews with Active Storage in Ruby on Rails 7](https://www.youtube.com/watch?v=nqAnftA8LbA)
- ### [Active Storage For Image Uploads | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=1cw6qO1EYGw)
- ### [Upload Multiple Images To Active Storage API! | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=U1IwoFo5pJA)

## [Handling Multiple File Uploads in Rails 7 | Ruby on Rails 7 Tutorial](https://www.youtube.com/watch?v=kisyDKKDB5g)
- [GIT:active_storage_multi_images](https://github.com/Deanout/active_storage_multi_images)
```
#########
# example
git clone git@github.com:Deanout/active_storage_multi_images.git

rails db:prepare

rails s

##############
# From scratch
#
rails _7.2.2.1_ new heidless_multi_image -j esbuild --css bootstrap --database=postgresql

rails db:prepare

rails g scaffold Post title:string content:text

rails active_storage:install

rails db:migrate

########
# ROUTES
#
vi config/routes.rb
--
  resources :posts do
    member do
      # remove_image_post_path(image)
      delete :remove_image
    end
  end
--

############
# MODEL
#
vi app/models/post.rb
--
  has_many_attached :images

--

############
# CONTROLLER
#
vi app/controllers/posts_controller.rb
--
  def remove_image
    @image = ActiveStorage::Attachment.find(params[:id])
    @image.purge_later
    redirect_back(fallback_location: request.referer)
  end

  private
  ... 
  
  # Only allow a list of trusted parameters through.
  def post_params
    params.require(:post).permit(:title, :content, images: [])
  end
--

############
# VIEW
#
vi app/views/posts/_form.html.erb
--
  <div>
    <%= form.label :images, "Upload images" %><br /><br />
    <%= form.file_field :images, multiple: true %>
  </div>
--

vi app/views/posts/_post.html.erb
--
  <% if post.images.attached? %>
    <% post.images.each do |image| %>
      <%= image_tag(image, height: 100) %>
      <%= link_to "Remove", remove_image_post_path(image), data: { turbo_method: :delete } %>
      <br />
    <% end %>
  <% end %>

--


```