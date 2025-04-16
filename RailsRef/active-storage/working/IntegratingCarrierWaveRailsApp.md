
## [Integrating CarrierWave in my Rails Application](https://samanbatool08.medium.com/integrating-carrierwave-in-my-rails-application-b8342b6384ea)
- ### [EXEMPLAR: rails_alpha_blog ](https://github.com/heidless-stillwater/rails_alpha_blog)
- ### [GIT: carrierwave](https://github.com/carrierwaveuploader/carrierwave)

```

#####
# GEM
#
bundle add image_processing
bundle add carrierwave
bundle add streamio-ffmpeg
bundle add mini_magick
bundle add fog 

#gem "fog-aws"
#gem 'dotenv-rails', '2.7.6'

####################
# Create IMAGE Class
#
rails g scaffold Image name:string picture:string user:references
rails db:migrate

vi config/initializers/carrier_wave.rb
--
    require 'carrierwave/storage/fog'
#    CarrierWave.configure do |config|
#      config.fog_provider = 'fog/aws'             # Use Fog for AWS compatibility
#      config.fog_credentials = {
#        provider:              'AWS',
#        aws_access_key_id:     ENV['AWS_ACCESS_KEY_ID'],       # Your AWS access key
#        aws_secret_access_key: ENV['AWS_SECRET_ACCESS_KEY'],   # Your AWS secret access key
#        region:                ENV['AWS_REGION'],              # The region of your Spaces bucket
#        endpoint:             ENV['AWS_ENDPOINT'] # The endpoint URL of your Spaces bucket
#      }
#      config.fog_directory  =  ENV['AWS_BUCKET']  # The name of your Spaces bucket 
#      config.fog_public     = true               # Set to true if you want to make uploaded files publicly accessible 
#      config.storage        = :fog               # Use fog storage 
#    end
--


##########
# UPLOADER
#
rails generate uploader Image

vi app/uploaders/image_uploader.rb
--
  # uncomment the following
...
include CarrierWave::MiniMagick
...
version :thumb do
process :resize_to_fill => [50, 50]
end
...
 def extension_allowlist
    %w(jpg jpeg gif png)
  end
...

###############
# MODEL - Image
#
vi app/model/image.rb
--
mount_uploader :image, ImageUploader

--

#######################
# Validating Image Size
#
vi app/model/image.rb
--
validates_processing_of :image
validate :image_size_validation

private

def image_size_validation
  errors[:image] << "should be less than 500KB" if image.size > 0.5.megabytes
end

--

```