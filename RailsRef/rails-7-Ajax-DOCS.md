
# [Ruby on Rails - AJAX](https://www.geeksforgeeks.org/ruby-on-rails-ajax/)
```
rails _7.2.2.1_ new geek_app -j esbuild --css bootstrap --database=postgresql

rails g controller welcome index

 

```





# [How to Use AJAX with Rails](https://reintech.io/blog/how-to-use-ajax-with-rails)
```
rails _7.2.2.1_ new ajax_rails_tutorial -j esbuild --css bootstrap --database=postgresql

cd ajax_rails_tutorial

rails db:prepare

rails generate scaffold Post title:string content:text
rails db:migrate

vi app/views/posts/index.html.erb
--
<%= form_with model: @post, remote: true do |form| %>
...
--

vi app/views/posts/_form.html.erb
--
<%= form.submit "Save", data: { disable_with: "Saving..." } %>
...
--

vi app/controllers/posts_controller.rb
--
# add line
format.js

--

vi app/views/posts/create.js.erb 
--
    // Remove any existing error messages
    $("#new_post .errors").remove();
    
    <% if @post.errors.any? %>
        // If there are errors, display them
        $("#new_post").prepend('&lt;div class="errors"&gt;&lt;%= j render "shared/errors", object: @post %&gt;&lt;/div&gt;');
    <% else %>
        // If the post was saved successfully, add it to the list and clear the form
        $("#posts tbody").prepend('&lt;%= j render @post %&gt;');
        $("#new_post")[0].reset();
    <% end %>

--


```