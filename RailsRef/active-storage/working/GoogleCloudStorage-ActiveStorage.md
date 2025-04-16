
## [Using Google Cloud Storage and Active Storage for a Rails App](https://medium.com/@daphnewang0826/using-google-cloud-storage-and-active-storage-for-a-rails-app-f66990ba10)
- ### [EXEMPLAR: GIT: rails-fin-track-7](git@github.com:heidless-stillwater/rails-fin-track-7.git)

```
######################
#### Google Cloud ####
######################

######
BUCKET
#
h-pfolio-2-rails-fin-trac-v2-bucket-0

h-pfolio-2-rails-photo-app-v2-bucket-0

#####################
# SERVICE ACCOUNT KEY
#
“APIs and Services” => “Credentials” 
--
669532112637-compute@developer.gserviceaccount.com

--

“APIs and Services” => “Credentials” => “Create credentials” => "Service Account" => "Default Compute Service Account"
=> 'Actions" => "Edit Service Account" => "Keys"
--
669532112637-compute@developer.gserviceaccount.com

"Add Key" => "Create New Key" => "JSON"

copy downloaded key from Downloads to config/secrets/
rename file to 'gcs.keyfile'
ie
mv h-pfolio-2-7a3b4cc4fc5a.json gcs.keyfile

--

###########
# GITIGNORE
#
vi .gitignore
--
/config/secrets/*

--


################
# STORAGE Config
#
vi config/storage.yml
--
# Remember not to checkin your GCS keyfile to a repository
google:
  service: GCS
  project: rails_fin_track_7
  credentials: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_keyfile) %>
  bucket: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_bucket) %>

#  bucket: your_own_bucket-<%= Rails.env %>

google_dev:
  service: GCS
  project: rails_fin_track_7
  credentials: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_keyfile) %>
  bucket: <%= Rails.application.credentials.dig(:gcs_storage, :gcs_bucket) %>

--

#############
# CREDENTIALS
#
EDITOR='subl --wait' ./bin/rails credentials:edit
--
gcp:
  db_password: mztptLRqRXBAdDmtPRolCqpRSJINnznnzXFIJwndbVNkifgTxZ
  db_database: rails-fin-trac-v2-db-0
  db_username: rails-fin-trac-v2-user-0
  db_instance_host: h-pfolio-2:europe-west1:rails-fin-trac-v2-instance-0
gcs_storage:
  gcs_bucket: h-pfolio-2-rails-fin-trac-v2-bucket-0
  gcs_keyfile: config/secrets/gcs.keyfile
yahoo_finance_api:
  api_key: e7f6e7637fmshab60958cd514432p13a37fjsn26bb0feacaee
finnhub_api:
  api_key: cvamkq1r01qsapma7mj0cvamkq1r01qsapma7mjg
stripe:
  test_publishable_key: pk_test_51PLjRQGNOeNKBTc3wA60vBCW1bdhLVc4Y8wP4NX5E1cXiAh2TNYYgGThPvkcEYakAoOS6VNjFlKEBeweZsRnfQ1E009yKKEh2B
  test_secret_key: sk_test_51PLjRQGNOeNKBTc3eZKbQPlaUWyanXsmwNlHyHkiCUL0AmaG69zx6h2TOOxvXHm6XphlGbQTgr2ML9gzyEmj9mS600VMH0777F
  webhook_secret: whsec_6856dc192142555126bb021b6a9e4ea56bca247adb76a71438d4bd6d4f55c6c4
sendgrid_mailer:
  user_name: apikey
  api_key_secret: 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
  mail_sender: support@naught4profit.co.uk
  domain: naught4profit.co.uk


--

##############
# ENVIRONMENTS
#
vi config/environments/development.rb
--
config.active_storage.service = :google_dev

--


#####
# GEM
#
bundle add google-cloud-storage

```