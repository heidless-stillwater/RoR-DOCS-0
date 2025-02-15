
#######################################################
#######################################################

# Database

### [gcp: Create a DATABASE](https://console.cloud.google.com/sql/instances/blog-demo-instance-1/databases?project=heidless-pfolio-deploy-5)


source ./config/.env-vars

echo $GCP_INSTANCE
echo $GCP_PROJECT_NAME
echo ' '
gcloud sql instances delete $GCP_INSTANCE \
--project=$GCP_PROJECT_NAME


## create Instance

echo $GCP_INSTANCE
echo $GCP_PROJECT_NAME
echo $GCP_DB_VERSION
echo $GCP_REGION
echo ' '
gcloud sql instances create $GCP_INSTANCE \
    --project $GCP_PROJECT_NAME \
    --database-version $GCP_DB_VERSION \
    --tier db-f1-micro \
    --region $GCP_REGION 


## set password on 'postgress' user

echo $GCP_INSTANCE
echo ' '
gcloud sql users set-password postgres \
--instance=$GCP_INSTANCE \
--password=postgres


## create DB
gcloud sql databases delete $GCP_DB_NAME \
    --instance $GCP_INSTANCE

echo 'create DB'
echo $GCP_DB_NAME
echo $GCP_INSTANCE
echo ' '
gcloud sql databases create $GCP_DB_NAME \
    --instance $GCP_INSTANCE

