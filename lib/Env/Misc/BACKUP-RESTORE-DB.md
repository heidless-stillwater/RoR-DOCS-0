
## [Cloud SQL(MySQL, PostgreSQL and Microsoft SQL Server) automatically backup and stores into the GCS bucket](https://medium.com/google-cloud/cloud-sql-mysql-postgresql-and-microsoft-sql-server-automatically-backup-and-stores-into-the-gcs-d01cf3677c67)


PROJECT:    heidless-pfolio-0
INSTANCE:   heidless-pfolio-0-backend-0-instance-0
DB:         heidless-pfolio-0-backend-0-db-0	
DB_USER:    heidless-pfolio-0-backend-0-user-0	


```
gcloud iam service-accounts list
--
572438747266-compute@developer.gserviceaccount.com
--

##########################
# ERROR: (gcloud.sql.export.sql) HTTPError 403: The service account does not have the required permissions for the bucket.
--
FIX: Export instance via Console to add permission. Then CLI works fine

GOOGLE Console->SQL-><INSTANCE>->Export

--
##########################
```

# Ruby Apps
```
gcloud init
--
heidless-ror-0
heidlessemail05@gmail.com
heidless-ror-0
europe-west2-a
--
```
## alpha-blog
```
export BK_PROJECT=heidless-ror-0
export BK_INSTANCE=alpha-blog-0-instance-0
export BK_DB=alpha-blog-0-db-0		
export BK_USER=alpha-blog-0-user-0	
export BK_BUCKET=gs://heidless-ror-0-alpha-blog-0-bucket-0
export BK_COMMENT='LatestSnapshot'
export BK_FILE=${BK_INSTANCE}-${BK_COMMENT}-${BK_TIMESTAMP}.gz
export BK_TIMESTAMP=`date +%s`

echo $BK_TIMESTAMP

gcloud sql export sql ${BK_INSTANCE} ${BK_BUCKET}/backups/${BK_FILE} \
--database=${BK_DB} \
--offload

# download backup
gcloud storage cp ${BK_BUCKET}/backups/${BK_FILE} .


gcloud storage cp gs://heidless-pfolio-0-bucket/images/* .


# upload backup

gcloud storage cp ${BK_FILE} ${BK_BUCKET}/backups/${BK_FILE}

####################################
# delete & re-create DB table
##
echo 're-create DB'
echo $BK_DB
echo $BK_INSTANCE
echo ' '
gcloud sql databases delete $BK_DB \
    --instance $BK_INSTANCE

## allow delete to settle - perhaps through Console

gcloud sql databases create $BK_DB \
    --instance $BK_INSTANCE

####################################

# restore (import) DB
gcloud sql import sql ${BK_INSTANCE} ${BK_BUCKET}/backups/${BK_FILE} \
--database=${BK_DB} \

```
########################################################################


# Heidless pfolio

```
gcloud init
--
heidless-pfolio-0
heidlessemail05@gmail.com
heidless-pfolio-0-backend-0-instance-0
europe-west2-a
heidless-pfolio-0-bucket


--
```
## 
```
timestamp=`date +%s`
MSG=2nd-DRAFT

gcloud sql export sql  heidless-pfolio-0-backend-0-instance-0 gs://heidless-pfolio-0-bucket/backups/heidless-pfolio-0-backend-0-instance-$MSG-$timestamp.gz \
--database=heidless-pfolio-0-backend-0-db-0	 \
--offload

# download backup
gcloud storage cp gs://heidless-pfolio-0-bucket/backups/heidless-pfolio-0-backend-0-instance-$MSG-$timestamp.gz .

# upload backup
gcloud storage cp TEST gs://heidless-pfolio-0-bucket/backups/TEST 


gcloud sql instances describe heidless-pfolio-0-backend-0-instance-0 | grep serviceAccountEmailAddress

gsutil iam ch serviceAccount:p520855347094-epa1wh@gcp-sa-cloud-sql.iam.gserviceaccount.com :objectAdmin \
gs://heidless-pfolio-0-bucket


gcloud sql import sql heidless-pfolio-0-backend-0-instance-0 gs://heidless-pfolio-0-bucket/backups/heidless-pfolio-0-bucket/backups/heidless-pfolio-0-backend-0-instance-$MSG-$timestamp.gz \
--database=heidless-pfolio-0-backend-0-db-0

```

