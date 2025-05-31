

#####################################
############# BACKEND ###############
#####################################

#####################
# New GCP Account & Project
#
```
# Billing Account
Select Project: h-pfolio-4
Billing->LinkToABillingAccount-My Billing Account

```


#####################
# GCP Login
#
```
gcloud init

```

#####################################
## generate password to 'dbpassword'.
```
# cat /dev/urandom | LC_ALL=C tr -dc '[:alpha:]'| fold -w 50 | head -n1 > dbpassword

```

## run INSTANCE script
```
source ./config/.env-vars
create_instance_dir='/home/heidless/projects/heidless-DOCS/RoR-DOCS-0/Admin'
ls ${create_instance_dir}
#
zsh ${create_instance_dir}/RoR-CREATE-INSTANCE

```

#########################
# SEED Data (IF REQUIRED)

### RE-CREATE DB
```
# pq: database "photo-app-0-db-0" is being accessed by other users
# CLOSE ALL TABS ACCESSING APP
#
#source ./config/.env-vars
#echo ' '

#echo 're-start instance: [${GCP_INSTANCE} : ${GCP_REGION}]'
#gcloud compute instances stop ${GCP_INSTANCE} \
#    --zone=${GCP_ZONE}
#gcloud compute instances stop ${GCP_INSTANCE} \
#    --zone=${GCP_ZONE}

#echo ' '
#echo 're-start DB instance'
#echo ' '
#gcloud compute instances start ${GCP_INSTANCE} \
#    --zone=europe-west1-d


source ./config/.env-vars
#
echo ' '
echo 're-create DB'
echo ' '
echo GCP_DB_NAME: ${GCP_DB_NAME}
echo GCP_INSTANCE: ${GCP_INSTANCE}

echo ' '
echo "### RESET DB ###"
echo ' '
gcloud sql databases delete ${GCP_DB_NAME} \
    --instance ${GCP_INSTANCE}

gcloud sql databases create ${GCP_DB_NAME} \
    --instance ${GCP_INSTANCE}

```

##################
# Enable APIs
#
```
APILibrary
->SecretManagerAPI
->CloudBuildAPI

```


# Secrets
```
EDITOR='subl --wait' ./bin/rails credentials:edit
--
gcp:
  db_password: kiCuPptHyyQujIQHEdpIGDirwnvbzswXjIfPHgXmQrRaMSnKqr
  db_database: react-on-rails-api-v1-db-0
  db_username: react-on-rails-api-v1-user-0
  db_instance_host: h-pfolio-4:europe-west2:react-on-rails-api-v1-instance-0

--
```

# utils - IF NEEDED
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

# review Secret
```
gcloud secrets describe ${GCP_SECRET_NAME}

# test - retrieve content of ${GCP_SECRET_NAME}
gcloud secrets versions access latest --secret ${GCP_SECRET_NAME} && echo ""


```

## setup gcp: secret access permissions
```
echo SECRET: ${GCP_SECRET_NAME}
echo PROJECT_ID: ${GCP_PROJECT_ID}
echo ' '

gcloud secrets add-iam-policy-binding ${GCP_SECRET_NAME} \
    --member serviceAccount:${GCP_PROJECT_ID}-compute@developer.gserviceaccount.com \
    --role roles/secretmanager.secretAccessor


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

echo ' '
echo "### DEPLOY app TO CLOUD RUN ###"
echo ' '
gcloud run deploy ${GCP_SERVICE_NAME} \
     --platform managed \
     --region ${GCP_REGION} \
     --image gcr.io/${GCP_PROJECT}/${GCP_SERVICE_NAME} \
     --add-cloudsql-instances ${GCP_PROJECT}:${GCP_REGION}:${GCP_INSTANCE} \
     --allow-unauthenticated

``` 


######################################
############# FRONTEND ###############
######################################

##########
# GIT Sync
# push to 'main'


############
# .env.development
# .env
#
--

# VITE_POSTS_API_URL=http://localhost:3000/api/v1/posts
# VITE_SEARCH_API_URL=http://localhost:3000/api/v1/search

VITE_POSTS_API_URL=https://react-on-rails-api-v1-svc-531028146137.europe-west2.run.app/api/v1/posts
VITE_SEARCH_API_URL=https://react-on-rails-api-v1-svc-531028146137.europe-west2.run.app/api/v1/search

--
```

########################
# GCP
#
CloudRun
-> Create Service
  --> Deploy Container->Service
    --> Continuously Deploy from a Repository
      --> Set up with Cloud Build
        Repository:
        heidless-stillwater/react-on-rails-frontend-v1
        Branch:
        ^main$
        Dockerfile:
        /Dockerfile
        <SAVE>
      -->
        Region:
        europe-west2
        Allow Unauthenticated invocations
      -> Cloud Build Trigger
        --> Container Port: 443
      -- 
      <CREATE>

```
