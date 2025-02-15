

# [Active Storage For Image Uploads | Deanin](https://www.youtube.com/watch?v=1cw6qO1EYGw)
```
LOCAL: 
/home/heidless/projects/heidless-ror/sandbox/active-storage-tut

rails _7.2.2.1_ new active_storage_attempt_1 -d postgresql
cd active_storage_attempt_1

rails generate scaffold pin title

rails active_storage:install
rails db:migrate

rails g action_text:install
rails db:migrate

vi config/environments/development.rb
--
  config.active_storage.variant_processor == :mini_magick

--

sudo apt install imagemagick
sudo apt install libvips

```

# [Using Google Cloud Storage and Active Storage for a Rails App](https://medium.com/@daphnewang0826/using-google-cloud-storage-and-active-storage-for-a-rails-app-f66990ba10)
```
# config/storage.yml
google:
  service: GCS
  project: Reviews-demo
  credentials: <%= ENV['GOOGLE_APPLICATION_CREDENTIALS'].as_json %>
  bucket: reviews_demo_bucket
google_dev:
  service: GCS
  project: Reviews-demo
  credentials: <%= Rails.root.join("config/secrets/reviews_demo.json") %>
  bucket: reviews_demo_bucket

```

# [Active Storage Overview](https://guides.rubyonrails.org/active_storage_overview.html#google-cloud-storage-service)
```
# Use bin/rails credentials:edit to set the GCS secrets (as gcs:private_key_id|private_key)
google:
  service: GCS
  credentials:
    type: "service_account"
    project_id: ""
    private_key_id: <%= Rails.application.credentials.dig(:gcs, :private_key_id) %>
    private_key: <%= Rails.application.credentials.dig(:gcs, :private_key).dump %>
    client_email: ""
    client_id: ""
    auth_uri: "https://accounts.google.com/o/oauth2/auth"
    token_uri: "https://accounts.google.com/o/oauth2/token"
    auth_provider_x509_cert_url: "https://www.googleapis.com/oauth2/v1/certs"
    client_x509_cert_url: ""
  project: ""
  bucket: your_own_bucket-<%= Rails.env %>

```

# [Set up Application Default Credentials](https://cloud.google.com/docs/authentication/provide-credentials-adc#how-to)


# [grrenlight/config/storage.yml](https://github.com/bigbluebutton/greenlight/blob/master/config/storage.yml)
```

# Remember not to checkin your GCS keyfile to a repository
google:
  service: GCS
  project: "<%= ENV['GCS_PROJECT'] %>"
  bucket: "<%= ENV['GCS_BUCKET'] %>"
  credentials:
    type: 'service_account'
    project_id: "<%= ENV['GCS_PROJECT_ID'] %>"
    private_key_id: "<%= ENV['GCS_PRIVATE_KEY_ID'] %>"
    private_key: "<%= ENV['GCS_PRIVATE_KEY']&.lines&.join("\\n") %>"
    client_email: "<%= ENV['GCS_CLIENT_EMAIL'] %>"
    client_id: "<%= ENV['GCS_CLIENT_ID'] %>"
    auth_uri: 'https://accounts.google.com/o/oauth2/auth'
    token_uri: 'https://accounts.google.com/o/oauth2/token'
    auth_provider_x509_cert_url: 'https://www.googleapis.com/oauth2/v1/certs'
    client_x509_cert_url: "<%= ENV['GCS_CLIENT_CERT'] %>"

```