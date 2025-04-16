
## [Integrating CarrierWave in my Rails Application](https://samanbatool08.medium.com/integrating-carrierwave-in-my-rails-application-b8342b6384ea)
- ### [EXEMPLAR: rails_alpha_blog ](https://github.com/heidless-stillwater/rails_alpha_blog)
```
####################
# Create IMAGE Class
#
rails g scaffold Image name:string picture:string user:references
rails db:migrate

vi config/initializers/carrier_wave.rb
--
#require 'carrierwave/orm/activerecord'

--

rails generate uploader Image


```