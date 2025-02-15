

# activate/configure cloud shell
```

gcloud config set project cat-cloud-run-0
gcloud config set compute/zone europe-west2

gcloud auth list

gcloud config list project

# enable APIs
Cloud Build API

```

# prep environments
```

git clone https://github.com/GoogleCloudPlatform/ruby-docs-samples.git

cd ruby-docs-samples/run/rails
bundle install

```

# prep backing services
```
DEVSHELL_PROJECT_ID=cat-cloud-run-0
INSTANCE_NAME=postgres-instance
DATABASE_NAME=mydatabase
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

 gcloud sql users create qwiklabs_user \
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
gcp:
  db_password: eTWCWFIZJgpCijWvAoTElFKXJwBNuceBylSkIKmHdWaQjRzjil
--

gcloud secrets delete cats_rails_secret

gcloud secrets create cats_rails_secret --data-file config/master.key

gcloud secrets describe cats_rails_secret

gcloud secrets versions access latest --secret cats_rails_secret

PROJECT_NUMBER=$(gcloud projects describe $DEVSHELL_PROJECT_ID --format='value(projectNumber)')

gcloud secrets add-iam-policy-binding cats_rails_secret \
  --member serviceAccount:$PROJECT_NUMBER-compute@developer.gserviceaccount.com \
  --role roles/secretmanager.secretAccessor

gcloud secrets add-iam-policy-binding cats_rails_secret \
  --member serviceAccount:$PROJECT_NUMBER@cloudbuild.gserviceaccount.com \
  --role roles/secretmanager.secretAccessor

```

## Connect Rails app to production database and storage
```
cat << EOF > .env
PRODUCTION_DB_NAME: $DATABASE_NAME
PRODUCTION_DB_USERNAME: qwiklabs_user
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

APP_NAME=myrubyapp
gcloud builds submit --config cloudbuild.yaml \
    --substitutions _SERVICE_NAME=$APP_NAME,_INSTANCE_NAME=$INSTANCE_NAME,_REGION=$REGION,_SECRET_NAME=cats_rails_secret --timeout=20m

 gcloud run deploy $APP_NAME \
     --platform managed \
     --region $REGION \
     --image gcr.io/$DEVSHELL_PROJECT_ID/$APP_NAME \
     --add-cloudsql-instances $DEVSHELL_PROJECT_ID:$REGION:$INSTANCE_NAME \
     --allow-unauthenticated \
     --max-instances=3



```

