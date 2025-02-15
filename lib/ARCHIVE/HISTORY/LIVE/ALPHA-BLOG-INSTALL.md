# Utils

- ## [Fix Rails Blocked Host Error with Docker](https://danielabaron.me/blog/rails-blocked-host-docker-fix/)

- ## [production ready rails/docker - docker-rails-example](https://github.com/nickjj/docker-rails-example)

- ## [Running Rails on Google Cloud](https://cloud.google.com/ruby/rails)
    - ### [Running Rails on the Cloud Run environment ](https://cloud.google.com/ruby/rails/run)
    - ### - [Quickstart: Deploy a Ruby service to Cloud Run](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-ruby-service)

# [Running Rails on the Cloud Run environment ](https://cloud.google.com/ruby/rails/run)


# local testing
```
rails server -e development

```

# activate/configure cloud shell
```
gcloud init

gcloud config set project ror-deploy-0
gcloud config set compute/zone europe-west2

gcloud auth list

gcloud config list project

```

# enable APIs
Cloud Build API
Compute Engine API

# prep environments
```
git clone https://github.com/heidless-stillwater/ruby-docs-app-suite.git

cd ruby-docs-app-suite/run/alpha-blog

bundle install

```

# prep backing services
```
PROJECT_NAME=ror-deploy-0
DEVSHELL_PROJECT_ID=$PROJECT_NAME
INSTANCE_NAME=alpha-blog-instance-0
DATABASE_NAME=alpha-blog-db-0
REGION=europe-west2
```

## Set up a Cloud SQL for PostgreSQL instance
```
gcloud sql instances create $INSTANCE_NAME \
  --database-version POSTGRES_12 \
  --tier db-g1-small \
  --region $REGION

gcloud sql databases create $DATABASE_NAME \
  --instance $INSTANCE_NAME

cat /dev/urandom | LC_ALL=C tr -dc '[:alpha:]'| fold -w 50 | head -n1 > dbpassword

gcloud sql users create alpha_blog_user \
  --instance=$INSTANCE_NAME --password=$(cat dbpassword)

```

## storage bucket
```
BUCKET_NAME=$DEVSHELL_PROJECT_ID-bucket
gsutil mb -l $REGION gs://$BUCKET_NAME

gsutil iam ch allUsers:objectViewer gs://$BUCKET_NAME

```

## secret config
```
rm config/*enc config/master.key

EDITOR="vi" bin/rails credentials:edit
--
secret_key_base: f29563712616ffb4d5caaf763298961fc2399bf7de037f157339496044677b1c60f8ce17cffbcf4123fb69d28a4e8e79ccb08a2741bf9cff9fd89f07e0c230b2
gcp:
  db_password: RumxdvacPxViYEDLhvnVErgSAsyUBvJIofENUqEQpUKEErSXOZ
--

gcloud secrets delete alpha-blog-secret

gcloud secrets create alpha-blog-secret --data-file config/master.key

gcloud secrets describe alpha-blog-secret

gcloud secrets versions access latest --secret alpha-blog-secret

PROJECT_NUMBER=$(gcloud projects describe $DEVSHELL_PROJECT_ID --format='value(projectNumber)')

gcloud secrets add-iam-policy-binding alpha-blog-secret \
  --member serviceAccount:$PROJECT_NUMBER-compute@developer.gserviceaccount.com \
  --role roles/secretmanager.secretAccessor

gcloud secrets add-iam-policy-binding alpha-blog-secret \
  --member serviceAccount:$PROJECT_NUMBER@cloudbuild.gserviceaccount.com \
  --role roles/secretmanager.secretAccessor

```

## Connect Rails app to production database and storage
```
cat << EOF > .env
PRODUCTION_DB_NAME: $DATABASE_NAME
PRODUCTION_DB_USERNAME: alpha_blog_user
CLOUD_SQL_CONNECTION_NAME: $DEVSHELL_PROJECT_ID:$REGION:$INSTANCE_NAME
GOOGLE_PROJECT_ID: $DEVSHELL_PROJECT_ID
STORAGE_BUCKET_NAME: $BUCKET_NAME
EOF

gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
    --member serviceAccount:$PROJECT_NUMBER@cloudbuild.gserviceaccount.com \
    --role roles/cloudsql.client

```

## Deploying the app to Cloud Run
```
RUBY_VERSION=$(ruby -v | cut -d ' ' -f2 | cut -c1-3)
sed -i "/FROM/c\FROM ruby:$RUBY_VERSION-buster" Dockerfile

APP_NAME=alpha-blog-svc
gcloud builds submit --config cloudbuild.yaml \
    --substitutions _SERVICE_NAME=$APP_NAME,_INSTANCE_NAME=$INSTANCE_NAME,_REGION=$REGION,_SECRET_NAME=alpha-blog-secret --timeout=20m

gcloud run deploy $APP_NAME \
    --platform managed \
    --region $REGION \
    --image gcr.io/$DEVSHELL_PROJECT_ID/$APP_NAME \
    --add-cloudsql-instances $DEVSHELL_PROJECT_ID:$REGION:$INSTANCE_NAME \
    --allow-unauthenticated \
    --max-instances=3

```

