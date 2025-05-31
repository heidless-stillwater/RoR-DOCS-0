

# Billing
```
# billing info for project
gcloud beta billing projects describe h-pfolio-2
--
billingAccountName: billingAccounts/0195BD-5694FF-2AFCB8
billingEnabled: true
name: projects/h-pfolio-2/billingInfo
projectId: h-pfolio-2
--

# billing account details
gcloud beta billing accounts describe 0195BD-5694FF-2AFCB8
--
currencyCode: GBP
--
displayName: My Billing Account
masterBillingAccount: ''
name: billingAccounts/0195BD-5694FF-2AFCB8
open: false
parent: ''

--

```

### LOCAL POSTGRES DB 
```
#######
# quck ref
#



# verify if DB is running
ps -ef | grep postgres

# find socket
sudo find /tmp/ -name .s.PGSQL.5432

# list all POSTGRES clusters
pg_lsclusters
--
Ver Cluster Port Status Owner    Data directory              Log file
14  main    5432 down   postgres /var/lib/postgresql/14/main /var/log/postgresql/postgresql-14-main.log

--

##################
# restart clusters
#
# format is pg_ctlcluster <version> <cluster> <action>
sudo pg_ctlcluster 14 main start

# restart PostgreSQL service
sudo service postgresql restart

##############################
# Configure APP DB environment
#
sudo su postges
psql

# drop db
DROP DATABASE portfolio;
  
# create db
CREATE DATABASE  portfolio;

# create user
CREATE USER arjuna11 WITH PASSWORD 'havana11';

# 'postgres' password
ALTER USER postgres with encrypted password 'postgres';

#####
UTILS
#
sudo systemctl status postgresql.service
sudo systemctl restart postgresql.service

```

# deploy checklist
- ## heidless-pfolio
  - ## frontend
    - ## [Deployment Guide - pfolio_frontend](joplin://x-callback-url/openNote?id=82fb6c3e591f4024b7578de8d0b455df)
    - ## GIT: [heidless-frontend](https://github.com/heidless-stillwater/pfolio-frontend)
      ```
      git clone git@github.com:heidless-stillwater/pfolio-frontend.git

      ```

  - ## backend
    - ## [Deployment Guide - pfolio_backend](joplin://x-callback-url/openNote?id=5b87c4ffe14145aabec57839a9c3c94b)
    - ## GIT: [heidless-frontend](https://github.com/heidless-stillwater/pfolio-backend)
      ```
      git clone git@github.com:heidless-stillwater/pfolio-backend.git

      ```

## prep for gCloud
```
# files
config/.env-vars
cloudbuild.yaml
#Dockerfile
#babel.config.js

vi config/database.yml
--
# [START cloudrun_rails_database]
production:
  <<: *default
  database: <%= Rails.application.credentials.dig(:gcp, :db_database) %>
  username: <%= Rails.application.credentials.dig(:gcp, :db_username) %>
  password: <%= Rails.application.credentials.dig(:gcp, :db_password) %>
  host: "<%= ENV.fetch("DB_SOCKET_DIR") { '/cloudsql' } %>/<%= Rails.application.credentials.dig(:gcp, :db_instance_host) %>"
# [END cloudrun_rails_database]

--

sudo apt-get install libxml2-dev
sudo apt-get install build-essential libcurl4-openssl-dev

```

## git
```
git config --global user.name "rob craig"

git config --global user.email "heidlessemail08@gmail.com"

```

## ruby version
```
#r_version='3.2.2'
#rvm install ${r_version} --with-openssl-dir=$HOME/.rvm/user

```

## gcloud
```
gcloud init

```

## app variabls
```
source ./config/.env-vars

```

## bundle
```
# enable 'pg' install
sudo apt install libpq-dev

bundle install

```

#####################################
## generate password to 'dbpassword'.
```
cat /dev/urandom | LC_ALL=C tr -dc '[:alpha:]'| fold -w 50 | head -n1 > dbpassword

```

## run INSTANCE script
```
source ./config/.env-vars
create_instance_dir='/home/heidless/projects/heidless-DOCS/RoR-DOCS-0/Admin'
ls ${create_instance_dir}
#
zsh ${create_instance_dir}/RoR-CREATE-INSTANCE

```

# configure SECRET Credentials
```

# make sure to delete previous config/credentials.yml.enc
rm config/credentials.yml.enc config/master.key

/bin/zsh --login
export PATH="/home/heidless/.rvm/gems/ruby-3.2.2/bin:$PATH"
rvm use --default 3.2.2

# subl - install
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/sublimehq-archive.gpg > /dev/null

echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

sudo apt-get update && sudo apt-get install libgl1-mesa-dev

sudo apt-get install sublime-text

```

#EDITOR='subl --wait' ./bin/rails credentials:edit --environment production

EDITOR='subl --wait' ./bin/rails credentials:edit

=======================================
gcp:
  db_password: trJfdEJtVlclBlHCqFWzuBQZOdtfOQbQExMkPtxmZgFCYSNjOk
  db_database: saas-project-app-v1-db-0
  db_username: saas-project-app-v1-user-0
  db_instance_host: h-pfolio-4:europe-west2:saas-project-app-v1-instance-0
gcs_storage:
  gcs_bucket: h-pfolio-4-saas-project-app-v1-bucket-0
  gcs_keyfile: config/gcs.keyfile

=======================================
gcp:
  db_password: kiCuPptHyyQujIQHEdpIGDirwnvbzswXjIfPHgXmQrRaMSnKqr
  db_database: react-on-rails-api-v1-db-0
  db_username: react-on-rails-api-v1-user-0
  db_instance_host: h-pfolio-4:europe-west2:react-on-rails-api-v1-instance-0
gcs_storage:
  gcs_bucket: h-pfolio-4-react-on-rails-api-v1-bucket-0
  gcs_keyfile: config/gcs.keyfile

=======================================
gcp:
  gcp_project_id: 58856964484
  gcp_storage_api_key: AIzaSyCSTf6bSfTcaEeE6nYIsaa44EVzwhRECBo
  gcp_storage_bucket: h-pfolio-1-active-storage-gcp-v1-bucket-0
  db_password: ggVWWFtbXVAOBHSOcLBuwMzArlIODkfQmRbxmUTmeWCqPpDSAh
  db_database: active-storage-gcp-v1-db-0
  db_username: active-storage-gcp-v1-user-0
  db_instance_host: h-pfolio-1:europe-west1:active-storage-gcp-v1-instance-0

=======================================
# rails-photo-app
#
gcp:
  db_password: kiCuPptHyyQujIQHEdpIGDirwnvbzswXjIfPHgXmQrRaMSnKqr
  db_database: react-on-rails-api-v1-db-0
  db_username: react-on-rails-api-v1-user-0
  db_instance_host: h-pfolio-2:europe-west1:react-on-rails-api-v1-instance-0
gcs_storage:
  gcs_bucket: h-pfolio-2-react-on-rails-api-v1-bucket-0
  gcs_keyfile: config/secrets/gcs.keyfile
  gcs_storage_prefix: photo-app-v1
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
  mail_sender: lockhart.r@gmail.com
  domain: naught4profit.co.uk

aws_credentials:
  S3_ACCESS_KEY: AKIAYUPGERUWQAUC4WXD
  S3_SECRET_KEY: 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
  S3_BUCKET: heidless-pdf-ninja
  AWS_REGION: eu-west-2   
iex_client:
  api_key: pk_808439936a93476fb7558256a9b6da5c
  secret_api_key: sk_7de5c9cf9cb74ec3ad57523bc62cc986
stripe:
  test_publishable_key: pk_test_51PLjRQGNOeNKBTc3wA60vBCW1bdhLVc4Y8wP4NX5E1cXiAh2TNYYgGThPvkcEYakAoOS6VNjFlKEBeweZsRnfQ1E009yKKEh2B
  test_secret_key: sk_test_51PLjRQGNOeNKBTc3eZKbQPlaUWyanXsmwNlHyHkiCUL0AmaG69zx6h2TOOxvXHm6XphlGbQTgr2ML9gzyEmj9mS600VMH0777F 
sendgrid_mailer:
  user_name: apikey
  api_key_secret: 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
  mail_sender: support@naught4profit.co.uk
  domain: naught4profit.co.uk


# Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
secret_key_base: 0d8cb44bdd6a27f074151e3d4b2f489c13fcbf1a4662c04958f7fd615db15a60ba3e07552c4932dd8b2fae0260f03f5285e0ac61df52a981c85c3fe188e99d21

```

```

## rebuild SECRET
```
source ./config/.env-vars
rebuild_secret_dir='/home/heidless/projects/heidless-DOCS/RoR-DOCS-0/Admin'
ls ${rebuild_secret_dir}
zsh ${rebuild_secret_dir}/RoR-RESET-SECRET

```
source ./config/.env-vars
echo ' '

echo SECRET: ${GCP_SECRET_NAME}
echo ' '
gcloud secrets delete ${GCP_SECRET_NAME}

```
## create 'secret'
```
source ./config/.env-vars
echo ' '

echo SECRET: ${GCP_SECRET_NAME}
echo ' '
gcloud secrets create ${GCP_SECRET_NAME} --data-file config/master.key

```

## Connect Rails app to production database and storage
```
echo ' '
echo "PRODUCTION_DB_NAME: $GCP_DB_NAME" > .env
echo "GOOGLE_PROJECT_ID: $GCP_PROJECT_ID" >> .env
echo "CLOUD_SQL_CONNECTION_NAME: $GCP_PROJECT:$GCP_REGION:$GCP_INSTANCE" >> .env
echo "PRODUCTION_DB_USERNAME: $GCP_DB_USER" >> .env
echo "STORAGE_BUCKET_NAME: $GCP_BUCKET" >> .env
echo "GOOGLE_APPLICATION_CREDENTIALS: gcs.keyfile" >> .env

#echo "AWS_ACCESS_KEY_ID: ${AWS_ACCESS_KEY_ID}" >> .env
#echo "AWS_SECRET_ACCESS_KEY: ${AWS_SECRET_ACCESS_KEY}" >> .env
#echo "AWS_S3_BUCKET: ${AWS_S3_BUCKET}" >> .env
#echo "AWS_S3_REGION: ${AWS_S3_REGION}" >> .env

echo "SENDGRID_API_KEY: $GCP_SENDGRID_API_KEY" >> .env
more .env

```

# if nokogiri an issue
# likely an issue with the Dockerfile
# https://hub.docker.com/_/ruby
# try a 'bullseye' image
# i.e. 3.4.2-bullseye

<!-- gem install nokogiri -v 1.16.0 -->
#bundle add nokogiri -v 1.16.6
#bundle install

<!-- vi Dockerfile
--
FROM ruby:3.4.2-bullseye
...
-- -->

# install nokogiri if not already in bundle
<!-- rm Gemfile.lock
gem install nokogiri
bundle lock --add-platform nokogiri-1.18.3-x86_64-linux-gnu
bundle install -->

<!-- bundle config set force_ruby_platform true -->

# 'node' issue
```
error postcss-cli@11.0.1: The engine "node" is incompatible with this module. Expected version ">=18". Got "14.21.3"

revise 'Dockerfile'
FROM ruby:3.2   # works

```

################
### RE-CREATE DB
###
```
# pq: database "photo-app-0-db-0" is being accessed by other users
# CLOSE ALL TABS ACCESSING APP
#
```
source ./config/.env-vars
#
echo ' '
echo "### RESET DB ###"
echo ' '
echo GCP_DB_NAME: ${GCP_DB_NAME}
echo GCP_INSTANCE: ${GCP_INSTANCE}
gcloud sql databases delete ${GCP_DB_NAME} \
    --instance ${GCP_INSTANCE}

gcloud sql databases create ${GCP_DB_NAME} \
    --instance ${GCP_INSTANCE}

```

#############################
# Submit the app to Cloud Run
```
source ./config/.env-vars
#
echo ' '
echo GCP_SERVICE_NAME: ${GCP_SERVICE_NAME}
echo GCP_INSTANCE: ${GCP_INSTANCE}
echo GCP_REGION: ${GCP_REGION}
echo GCP_SECRET_NAME: ${GCP_SECRET_NAME}

echo ' '
echo "### SUBMIT app TO REGISTRY ###"
echo ' '
gcloud builds submit --config cloudbuild.yaml \
    --substitutions _SERVICE_NAME=${GCP_SERVICE_NAME},_INSTANCE_NAME=${GCP_INSTANCE},_REGION=${GCP_REGION},_SECRET_NAME=${GCP_SECRET_NAME} 

```

#############################
# Deploy the app to Cloud Run
```
source ./config/.env-vars
echo ' '

echo GCP_SERVICE_NAME: ${GCP_SERVICE_NAME}
echo GCP_REGION: ${GCP_REGION}
echo GCP_PROJECT: ${GCP_PROJECT}
echo GCP_INSTANCE: ${GCP_INSTANCE}
echo GCP_SECRET_NAME: ${GCP_SECRET_NAME}

gcloud run services describe ${GCP_SERVICE_NAME}

echo ' '
echo "### DEPLOY app TO CLOUD RUN ###"
echo ' '
gcloud run deploy ${GCP_SERVICE_NAME} \
     --platform managed \
     --region ${GCP_REGION} \
     --image gcr.io/${GCP_PROJECT}/${GCP_SERVICE_NAME} \
     --add-cloudsql-instances ${GCP_PROJECT}:${GCP_REGION}:${GCP_INSTANCE} \
     --allow-unauthenticated \
     --memory 1G

``` 

SaaS-Business.jpg
SaaS_01.jpg

###########
# UTILITIES
#
# reset memory limit
```
gcloud run services update ${GCP_SERVICE_NAME} --memory 2Gi

```

#############################
## CLOUD RUN: domain mappings
gcloud domains list-user-verifiedls


DOMAIN=westgreenconnection.co.uk
echo DOMAIN: ${DOMAIN}
echo GCP_SERVICE_NAME: ${GCP_SERVICE_NAME}
gcloud beta run domain-mappings create --service ${GCP_SERVICE_NAME} --domain ${DOMAIN} --force-override

---

```
####################################################
# cloud-sql-proxy 
## if needed...
####################################################

## start proxy
##curl -o cloud-sql-proxy https://storage.googleapis.com/cloud-sql-connectors/cloud-sql-proxy/v2.11.0/cloud-sql-proxy.linux.amd64
##chmod +x cloud-sql-proxy

#./cloud-sql-proxy -instances=$GCP_PROJECT_ID:$GCP_REGION:$GCP_INSTANCE=tcp:3306 -credential_file=heidless-ror-0-658f2b5978e6 &

```
#export GOOGLE_CLOUD_PROJECT=heidless-ror-0
#export USE_CLOUD_SQL_AUTH_PROXY=true
#export CLOUDRUN_SERVICE_URL=https://heidless-pfolio-deploy-7@appspot.gserviceaccount.com

```

### enable cloud proxy
```
#./cloud-sql-proxy --credentials-file ./heidless-ror-0-658f2b5978e6.json \
#--port 1234 heidless-ror-0:europe-west2:fin-tracker-0-instance-0 

# kill & restart - IF address already in use
#sudo lsof -i -P -n | grep LISTEN
#kill -9 <PID>

```
rails s     # fails with webpack issues

bundle exec rails webpacker:install     # then retry

npm install --save-dev @babel/plugin-transform-private-methods
npm install --save-dev @babel/core@^7.0.0-0
npm install --save-dev @babel/plugin-proposal-private-methods

npm i fsevents@2.2.1


    create_table :articles do |t|
      t.text :title
      t.text :description
      t.timestamps

rails generate scaffold Article title:text description:text user:references
rails g bootstrap:themed Articles
article = Article.new(title: "third article", description: "description of third article", user_id: 1)

<%= link_to t('.back', :default => t("helpers.links.back")),
              articles_path, :class => 'btn btn-default'  %>
<%= link_to t('.edit', :default => t("helpers.links.edit")),
              edit_article_path(@article), :class => 'btn btn-default' %>
<%= link_to t('.destroy', :default => t("helpers.links.destroy")),
              article_path(@article),
              :method => 'delete',
              :data => { :confirm => t('.confirm', :default => t("helpers.links.confirm", :default => 'Are you sure?')) },
              :class => 'btn btn-danger' %>


rails generate scaffold Image name:string picture:string user:references


rails generate scaffold Pdf name:string description:string pdfdoc:string user:references

rails db:migrate

rails g bootstrap:themed Pdfs

user.rb
--
has_many :pdfs

--

rails generate uploader Pdfdoc




