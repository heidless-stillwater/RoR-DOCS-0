
# utils
```
#grep -Rnw '/path/to/somewhere/' -e 'pattern'

grep -Rnw '.' -e 'PROJECT'

```


# [Using Ruby on Rails with Cloud SQL for PostgreSQL on Cloud Run](https://www.cloudskillsboost.google/focuses/20049?parent=catalog)

## [Event tokens](https://support.google.com/qwiklabs/answer/9120818?hl=en)
- required to access lab

## in cloud shell
```
#git clone https://github.com/GoogleCloudPlatform/ruby-docs-samples.git

git clone https://github.com/heidless-stillwater/alpha-blog.git

cd ruby-docs-samples/run/alpha-blog
bundle install
```

## prep backing services - DB
```
#INSTANCE_NAME=postgres-instance
#DATABASE_NAME=mydatabase
#REGION=us-central1

gcloud auth list
--
ACCOUNT: heidlessemail04@gmail.com
--


gcloud config set project heidless-pfolio-deploy-7
gcloud config set compute/zone europe-west2

gcloud config list project
--
heidless-pfolio-deploy-5
--


DEVSHELL_PROJECT_ID=heidless-pfolio-deploy-5
APP_NAME=alpha-blog
APP_SECRET=$APP_NAME_secret
INSTANCE_NAME=alpha-blog-instance
DATABASE_NAME=alpha-blog-db
DB_USERNAME=alpha-blog-db-user
REGION=europe-west2

gcloud sql instances create $INSTANCE_NAME \
  --database-version POSTGRES_12 \
  --tier db-g1-small \
  --region $REGION

gcloud sql databases create $DATABASE_NAME \
  --instance $INSTANCE_NAME

cat /dev/urandom | LC_ALL=C tr -dc '[:alpha:]'| fold -w 50 | head -n1 > dbpassword

gcloud sql users create $DB_USERNAME \
  --instance=$INSTANCE_NAME --password=$(cat dbpassword)

```

## prep backing services - STORAGE BUCKET
```
BUCKET_NAME=$DEVSHELL_PROJECT_ID-ruby
gsutil mb -l $REGION gs://$BUCKET_NAME

gsutil iam ch allUsers:objectViewer gs://$BUCKET_NAME

```

## prep backing services - SECRET
```
EDITOR="vi" bin/rails credentials:edit
--
# aws:
#   access_key_id: 123
#   secret_access_key: 345

# Used as the base secret for all MessageVerifiers in Rails, including the one protecting cookies.
secret_key_base: af9a4995ff8495e9a6bd5a035e674d1dac0fae06ac97577293fcee14147c07ba70a48db9497d3249af0896e1897ae1e12bc5b546b7c5139c333abde15ff43d20
gcp:
  db_password: qkMzYnHcFJQMnwfEusxiwArwLHzvsCtNDrRksmwyiCGdQgfQYT
--

gcloud secrets delete alpha_blog_secret_0

gcloud secrets create alpha_blog_secret_0 --data-file config/master.key

gcloud secrets describe alpha_blog_secret_0

gcloud secrets versions access latest --secret alpha_blog_secret_0

PROJECT_NUMBER=$(gcloud projects describe $DEVSHELL_PROJECT_ID --format='value(projectNumber)')

gcloud secrets add-iam-policy-binding alpha_blog_secret_0 \
  --member serviceAccount:$PROJECT_NUMBER-compute@developer.gserviceaccount.com \
  --role roles/secretmanager.secretAccessor

gcloud secrets add-iam-policy-binding alpha_blog_secret_0 \
  --member serviceAccount:$PROJECT_NUMBER@cloudbuild.gserviceaccount.com \
  --role roles/secretmanager.secretAccessor

```

## Connect Rails app to production database and storage
```
cat << EOF > .env
PRODUCTION_DB_NAME: $DATABASE_NAME
PRODUCTION_DB_USERNAME: $DB_USERNAME
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

#APP_NAME=myrubyapp
gcloud builds submit --config cloudbuild.yaml \
    --substitutions _SERVICE_NAME=$APP_NAME,_INSTANCE_NAME=$INSTANCE_NAME,_REGION=$REGION,_SECRET_NAME=alpha_blog_secret_0 --timeout=20m

 gcloud run deploy $APP_NAME \
     --platform managed \
     --region $REGION \
     --image gcr.io/$DEVSHELL_PROJECT_ID/$APP_NAME \
     --add-cloudsql-instances $DEVSHELL_PROJECT_ID:$REGION:$INSTANCE_NAME \
     --allow-unauthenticated \
     --max-instances=3

```


The request uri `ht3ps://index.rubygems.org/versions` has an invalid scheme (`ht3ps`). Did you mean `http` or `https`?