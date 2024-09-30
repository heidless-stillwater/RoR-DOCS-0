# postgres migration notes

### application.html.erb
```
<%= javascript_include_tag "/javascript/application.js", "data-turbo-track": "reload", defer: true %>
```

### postgres
```
psql -U heidless -c 'SHOW config_file'
--
 /etc/postgresql/14/main/postgresql.conf
--

CREATE DATABASE alpha_blog_user;

ALTER USER heidless WITH PASSWORD 'password';

```
### @hotwirwd
[turbo-rails](https://github.com/hotwired/turbo-rails)

### secret_key_base
```
# generate new secret_key_base
rake secret
--
8da0814c0203e3035993040da4ea8b46246fc0d18db8b6d6d14b1fa2353a33d71cec8b62dfb48565e2dc66943cc94fb4015707420dfc732df273314753ec7835
--

# set production credentials
EDITOR='subl --wait' rails credentials:edit


#EDITOR='subl --wait' bin/rails credentials:edit --environment production
```
secret_key_base: f8ed1ec2a74848927eb8f32d6caa49364c3986b57e65fd945261ae1bcec9c78ebb464ecbe02fe9fb67419fc00f514fa0a88af4e5a7ca90c0f568bb17a46c022d
    db_password: 


# config/credentials/production.yml.enc

```


### linux
```
find / -name "postgresql.conf" -not -path "*/mnt*"
find -name "postgresql.conf"

find . -path ./misc -prune -o -name '*.txt' -print

find / -name "postgresql.conf" ! -path '*/mnt/*'

grep -Rnw '/path/to/somewhere/' -e 'pattern'

grep -Rnw '.' -e 'rvm'



```




# Utils

- ## [Fix Rails Blocked Host Error with Docker](https://danielabaron.me/blog/rails-blocked-host-docker-fix/)

- ## [production ready rails/docker - docker-rails-example](https://github.com/nickjj/docker-rails-example)

- ## [Running Rails on Google Cloud](https://cloud.google.com/ruby/rails)
    - ### [Running Rails on the Cloud Run environment ](https://cloud.google.com/ruby/rails/run)
    - ### - [Quickstart: Deploy a Ruby service to Cloud Run](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-ruby-service)


# [Running Rails on the Cloud Run environment ](https://cloud.google.com/ruby/rails/run)

### Cloning the Rails app
```
#git clone https://github.com/GoogleCloudPlatform/ruby-docs-samples.git

#cd ruby-docs-samples/run/rails

git clone https://github.com/heidless-stillwater/rails-cat-album-gcloud-deploy.git

cd rails-cat-album-gcloud-deploy/run/rails

cd gcloud-run-ruby-proto/run/cat-photo-album

bundle install
```


## Generate RAILS_MASTER_KEY - If REQUESTED
```
# refreshes 'config/master.key' & 'config/credentials.enc' 
EDITOR='subl --wait' bin/rails credentials:edit --environment development

EDITOR='subl --wait' bin/rails credentials:edit --environment production

```

## dotenv environment
### [Setting up Dotenv in Rails](https://onlyoneaman.medium.com/setting-up-dotenv-in-rails-a8ce1c69e03d)
```
# initialise .env
touch .env

```

## create SQL instance

### [instances](https://console.cloud.google.com/sql/instances?project=heidless-pfolio-deploy-5)

## USEFUL IF NEEDED
```
# enable/diable deletion protection
gcloud sql instances patch cat-photo-album-0 \
    --deletion-protection

```

## prep OS
```
<!-- cd ./run/cat-photo-album
yarnpkg add -W caniuse-lite
sudo npm install -g n
sudo n latest
npx update-browserslist-db@latest
sudo apt install apt-utils -->


# prep OS
sudo apt install apt-utils
```


################## CONFIGURE APP ###################
# setup APP_name
```
#export APP_NAME=alpha-blog
#export APP_NAME=ab-test
#export APP_NAME=cat-photo-album
#export APP_NAME=rails
export APP_NAME=cat-album
echo $APP_NAME

```

# Database

### [gcp: Create a DATABASE](https://console.cloud.google.com/sql/instances/blog-demo-instance-1/databases?project=heidless-pfolio-deploy-5)

```
gcloud sql instances create $APP_NAME-instance-0 \
    --database-version POSTGRES_12 \
    --tier db-f1-micro \
    --region europe-west2

```

### create USER
### [gcp: create user](https://console.cloud.google.com/sql/instances/blog-demo-instance-1/users?project=heidless-pfolio-deploy-5)

```
gcloud sql databases create $APP_NAME-db-0 \
    --instance $APP_NAME-instance-0

```

### gcp: password
```
# generate password to 'dbpassword'.
# in PRODUCTION USE 'dbpassword'. 

cat /dev/urandom | LC_ALL=C tr -dc '[:alpha:]'| fold -w 50 | head -n1 > dbpassword

---
gcloud sql users create $APP_NAME-user-0 \
   --instance=$APP_NAME-instance-0 --password=$(cat dbpassword)

```
### Set up a Cloud Storage bucket
### [storage bucket](https://console.cloud.google.com/storage/browser?referrer=search&project=heidless-pfolio-deploy-5&prefix=&forceOnBucketsSortingFiltering=true)

```
gsutil mb -l europe-west2 gs://heidless-pfolio-deploy-5-$APP_NAME-bucket-0
```

### [bucket permissions]()
```
gsutil iam ch allUsers:objectViewer gs://heidless-pfolio-deploy-5-$APP_NAME-bucket-0

```

## Secret Mgr
### Create encrypted credentials file and store key as Secret Manager secret

### install sublime - IF NEEDED
```
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/sublimehq-archive.gpg > /dev/null

echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

sudo apt-get update
```

### ubuntu window - if NEEDED
```
sudo apt-get install sublime-text
```

### setup gcp:credentials
### sets up encryped crtedentials for use in live

```
# make sure to delete previous config/credentials.yml.enc
rm config/credentials.yml.enc config/master.key
---
# get password from './dbpassword'
VyAAHPfWIEzYkcOOmbdupWxYfSqYKWxvRCHJGzUKLZWbuYuuUE
---

# password value in file config/credentials.yml.enc
EDITOR='subl --wait' ./bin/rails credentials:edit

---
# potentially setup other definitions: 
#   db_name_production
#   cloud_sql_connection_name
#   google_project_id
#   storage_bucket_id
---
secret_key_base: GENERATED_VALUE
gcp:
  db_password: VyAAHPfWIEzYkcOOmbdupWxYfSqYKWxvRCHJGzUKLZWbuYuuUE
```

```
# utils - IF NEEDED
gcloud secrets delete $APP_NAME-secret-0

```

### create gcp:secret
```
gcloud secrets create $APP_NAME-secret-0 --data-file config/master.key
```

### describe Secret
```
gcloud secrets describe $APP_NAME-secret-0

gcloud secrets versions access latest --secret $APP_NAME-secret-0
--
84915be09e2f6e83df81c0594eded8e7%                                                                                                  
--

gcloud projects describe heidless-pfolio-deploy-5 --format='value(projectNumber)'
---
110223146514
---

```

### setup gcp: secret access permissions

## COMPUTE access to secrets
```
gcloud secrets add-iam-policy-binding $APP_NAME-secret-0 \
    --member serviceAccount:110223146514-compute@developer.gserviceaccount.com \
    --role roles/secretmanager.secretAccessor

## CLOUD BUILd access to secrets
gcloud secrets add-iam-policy-binding $APP_NAME-secret-0 \
    --member serviceAccount:110223146514@cloudbuild.gserviceaccount.com \
    --role roles/secretmanager.secretAccessor

```

## Connect Rails app to production database and storage
### .env
```
cd PROJECT_ROOT
touch .env
---
PRODUCTION_DB_NAME: alpha-blog-db-0
PRODUCTION_DB_USERNAME: alpha-blog-user-0
CLOUD_SQL_CONNECTION_NAME: heidless-pfolio-deploy-5:europe-west2:alpha-blog-instance-0
GOOGLE_PROJECT_ID: heidless-pfolio-deploy-5
STORAGE_BUCKET_NAME: heidless-pfolio-deploy-5-alpha-blog-bucket-0
```

### Grant Cloud Build access to Cloud SQL
```
gcloud projects add-iam-policy-binding heidless-pfolio-deploy-5 \
    --member serviceAccount:110223146514@cloudbuild.gserviceaccount.com \
    --role roles/cloudsql.client

```

# Deploying the app to Cloud Run

```
gcloud builds submit --config cloudbuild.yaml \
    --substitutions _SERVICE_NAME=$APP_NAME-svc,_INSTANCE_NAME=$APP_NAME-instance-0,_REGION=europe-west2,_SECRET_NAME=$APP_NAME-secret-0

---

gcloud run deploy $APP_NAME-svc \
     --platform managed \
     --region europe-west2 \
     --image gcr.io/heidless-pfolio-deploy-5/$APP_NAME-svc \
     --add-cloudsql-instances heidless-pfolio-deploy-5:europe-west2:$APP_NAME-instance-0 \
     --allow-unauthenticated

---

Service [cat-album-svc] revision [cat-album-svc-00001-mh6] has been deployed and is serving 100 percent of traffic.

Service URL: https://cat-album-svc-pvqvpcgdcq-nw.a.run.app


# service account key mgmt
- ### [Create and delete service account keys](https://cloud.google.com/iam/docs/keys-create-delete)

```

gcloud iam service-accounts keys create $APP_NAME-key \
    --iam-account=110223146514-compute@developer.gserviceaccount.com

```

```
export SA_KEY_NAME=$APP_NAME-key
PORT=8080 && docker run --rm \
    -p 8080:${PORT} -e PORT=${PORT} \
    -e GOOGLE_APPLICATION_CREDENTIALS=/tmp/keys/${SA_KEY_NAME}.json \
    -v $GOOGLE_APPLICATION_CREDENTIALS:/tmp/keys/${SA_KEY_NAME}.json:ro \
    -v $PWD:/usr/src/app $APP_NAME
```


