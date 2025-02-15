
## [Public APIs](https://publicapis.dev/)
- ## [Science & Math](https://publicapis.dev/category/science-and-math)
  - ## [{ NASA APIs }](https://api.nasa.gov/?ref=public_apis)
  ```
  API Key:
  DAbe8pRQBBRGzIhyQpaZsKymOFr9QHfjs2xpqFAp

  EXAMPLE: 
  https://api.nasa.gov/planetary/apod?api_key=DAbe8pRQBBRGzIhyQpaZsKymOFr9QHfjs2xpqFAp

  ```
    - ## [Astronomy Picture of the Day: APOD]
    https://apod.nasa.gov/apod/image/2408/GloryFog_label.png"



# [railsWeatherApp](https://medium.com/@bamnelkarvivek/how-i-made-a-simple-weather-app-in-ruby-on-rails-309cdced25aa)
```

rails _7.1.3.4_ new weatherapp --force -d=postgresql

Gemfile
--
rails generate controller Weather

--

bundle install

app/services/weather_api.rb
--
require 'httparty'
class WeatherApi
  include HTTParty
  base_uri 'https://api.openweathermap.org/data/2.5'
  def initialize(api_key)
    @options = { query: { appid: api_key } }
  end
  def weather_by_city(city)
    self.class.get("/weather?q=#{city}", @options)
  end
end

--

rails generate controller Weather

# generate API key
https://home.openweathermap.org/api_keys
--
name:
weather-api-key

key:
e2a994ea181a611c5d0b3932bcded73d

--

app/controller/weather_controller.rb
--
class WeatherController < ApplicationController
  def index
    api_key = 'e2a994ea181a611c5d0b3932bcded73d'
    api = WeatherApi.new(api_key)
    @weather = api.weather_by_city('New York')
  end
end

--

app/views/weather/index.html.erb
--
<% if @weather['main'] %>
 <p>Current Weather in New York City </p>
 <p>Temperature: <%= @weather['main']['temp'] - 273.15 %>°C></p>
 <p>Humidity: <%= @weather['main']['humidity'] %> </p>
 <p>Wind Speed: <%= @weather['wind']['speed'] * 3.6 %> km/h </p>
<% else %>
 <code> Please make sure you've updated your "api-key" in controllers/weatherController.rb </code>
<% end %>

--

config/routes.rb
--
Rails.application.routes.draw do
  root "weather#index"
end

--

rails s


rails generate scaffold Weather location:string temperature:float humidity:float windspeed:float

rails generate scaffold Apod name:string picture:string





```

# [Making Ruby HTTP Request Easy With Faraday](https://coreyscorner.medium.com/making-ruby-http-request-easy-with-faraday-bd8abb2abc3f)
- ## [API Requests In Rails With The Faraday Gem | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=nAVM3Ywck0A)
```

cd video

############### API #################

rails new api --api

cd api

rails g scaffold Post title views:integer

rails db:migrate

Gemfile
--
# Use Rack CORS for handling Cross-Origin Resource Sharing (CORS), making cross-origin Ajax possible
gem "rack-cors"

--

bundle install

config/initializers/cors.rb
--
# replace:    origins "example.com"
# with:       origins "*"

--

rails s -p 3001


############### CLIENT #################

cd video

rails new client

cd client

rails g controller pages home

bundle add faraday

rails s -p 3000










```


# [Integrating External API Ruby on Rails](https://rahman-saima.medium.com/integrating-external-api-ruby-on-rails-76a05ef8b0e8)
- ## [Astronomy Picture of the Day: APOD](https://apod.nasa.gov/apod/astropix.html)
```
APP NAME:
apod_api

API KEY:
DAbe8pRQBBRGzIhyQpaZsKymOFr9QHfjs2xpqFAp

rails new apod_api --force -d=postgresql

rails generate scaffold Apod name:string description:text url:text

https://api.nasa.gov/planetary/apod?api_key=DAbe8pRQBBRGzIhyQpaZsKymOFr9QHfjs2xpqFAp&count=5


```







## [The Open Movie Database: OMDb API](https://www.omdbapi.com/)
```
API Key:
b122acf5

http://www.omdbapi.com/?apikey=b122acf5&




```


# [The Movie Database (TMDB) API](https://developer.themoviedb.org/reference/intro/getting-started)
```
heidless
b28zLs@wFicTBXZ
lockhart.r@gmail.om

API:
--
App Name:
heidless-prototype-0

App URL:
https://heidless.co.uk



--

```


## [The Ultimate List of Fun APIs For Your Next Coding Project](https://developer.vonage.com/en/blog/the-ultimate-list-of-fun-apis-for-your-next-coding-project)

- ## [Polygon.io API](https://polygon.io/docs/stocks?adobe_mc=MCMID%3D19358115655885526433086505052002768474%7CMCORGID%3DA8833BC75245AF9E0A490D4D%2540AdobeOrg%7CTS%3D1722610586)
- ## financial tracking
```
# Signup with github

# API keys
Default
l6U41glk2ByuNyeNMxIiQZC2a_BZOKMn

```

## [QR Tag API](https://www.qrtag.net/api/?adobe_mc=MCMID%3D19358115655885526433086505052002768474%7CMCORGID%3DA8833BC75245AF9E0A490D4D%2540AdobeOrg%7CTS%3D1722610768)
- embed QR code images

## [Clarifai API](https://docs.clarifai.com/?adobe_mc=MCMID%3D19358115655885526433086505052002768474%7CMCORGID%3DA8833BC75245AF9E0A490D4D%2540AdobeOrg%7CTS%3D1722610768)
- ## Clarifai provides a complete platform to deploy, maintain, and manage your AI models

# nasa 
- ## [{ NASA APIs }](https://api.nasa.gov/)



# weather





