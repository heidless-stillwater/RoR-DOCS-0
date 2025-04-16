## [Rapid: Free Public APIs for Developers](https://rapidapi.com/collection/list-of-free-apis)
- ### [YH Finance](https://rapidapi.com/sparior/api/yahoo-finance15/playground/apiendpoint_9d15a1cc-d1be-4a69-9622-1f59ab68183c)


## [API Requests In Rails With The Faraday Gem | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=nAVM3Ywck0A)
- ### [GIT:API Requests In Rails With The Faraday Gem](https://github.com/Deanout/faraday)
```
##############
# init project
#
mkdir video

cd video

rails _7.2.2.1_ new api  --api

cd api

rails g scaffold post title views:integer

######
# CORS
#
vi Gemfile
--
# uncomment the following
gem "rack-cors"

--
bundle install

##############
# initializers
#
vi config/initializers/cors.rb
--
# uncomment & substitute 'example.com' with '*'

#############################
# CLIENT
#
cd video

rails _7.2.2.1_ new client

cd client

rails g controller pages home 

bundle add faraday

vi config/routes.rb
--
  root "pages#home"
  post 'create-post', to: 'pages#create_post'

--

vi pages_controller.rb
--
  def home
    @yfs = get_yf
  end

  def get_yf
    require 'uri'
    require 'net/http'
    
    url = URI("https://yahoo-finance15.p.rapidapi.com/api/v1/markets/stock/history?symbol=AAPL&interval=5m&diffandsplits=false")
    
    http = Net::HTTP.new(url.host, url.port)
    http.use_ssl = true
    
    request = Net::HTTP::Get.new(url)
    request["x-rapidapi-key"] = 'e7f6e7637fmshab60958cd514432p13a37fjsn26bb0feacaee'
    request["x-rapidapi-host"] = 'yahoo-finance15.p.rapidapi.com'
    
    response = http.request(request)
    puts response.read_body
    JSON.parse(response.read_body)
  end

--

vi views/wecome/index.html.erb
--
<%= flash[:alert] if flash[:alert] %>
<%= flash[:notice] if flash[:notice] %>

<h1>Pages#home</h1>
<p>Find me in app/views/pages/home.html.erb</p>

<%= form_with url: create_post_path, method: :post do |form| %>
  <%= form.text_field :title %>
  <%= form.submit %>
<% end %>

<% @posts.each do |post| %>
  <p><%= post["title"] %></p>
  <em><%= post["views"] %></em>
  <%= button_to "Delete", destroy_post_path(post["id"]), method: :delete %>
<% end %>

<h2><%= @yfs["meta"]["symbol"] %></h2>
<% @yfs["body"].each do |yf| %>
  <ul>
    <li><%= yf[1]["date"] %> : <%= yf[1]["open"] %> : <%= yf[1]["high"] %> : <%= yf[1]["low"] %> : <%= yf[1]["close"] %></li>
  </ul>
<% end %>


--


```

#################
# response / JSON
```
response
response.body
response.body.to_json

@post = JSON.parse(response.body)
@post["title"]

```
