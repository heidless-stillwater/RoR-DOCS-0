
## [Cloud SQL(MySQL, PostgreSQL and Microsoft SQL Server) automatically backup and stores into the GCS bucket](https://medium.com/google-cloud/cloud-sql-mysql-postgresql-and-microsoft-sql-server-automatically-backup-and-stores-into-the-gcs-d01cf3677c67)


##################################################################################
##################################################################################

```
gcloud init

https://alpha-blog-0-svc-d57dc7eqba-ew.a.run.app/
https://alpha-blog-0-svc-d57dc7eqba-nw.a.run.app/


#############################
#
# GIT: reinstate 'better_branch' to 'main' branch
git checkout main

git reset --hard better_branch

git push -f origin main

#############################

echo '######################'
echo 'set ENV'
source config/.env-vars

cd ./BACKUPS/<BACKUP DIR>
source ../../config/.env-vars

```

## set SERVICE ACCOUNT Permissons
```
--
# set permissoons on INSTANCE service account
echo ' '
echo '#################################'
echo '#### SET PERMISSIONS - objectAdmin & legacyBucketWriter'
echo GCP_INSTANCE: ${GCP_INSTANCE}
export SQL_SVC_ACC=`gcloud sql instances describe ${GCP_INSTANCE} | grep serviceAccountEmailAddress`
echo SQL_SVC_ACC: ${SQL_SVC_ACC}

export DB_SVC_ACCOUNT=`echo ${SQL_SVC_ACC}|sed 's/.* //g'`
echo DB_SVC_ACCOUNT: ${DB_SVC_ACCOUNT}

###############################
# set PERMISSION on Svc Account
echo DB_SVC_ACCOUNT: ${DB_SVC_ACCOUNT}
echo GCP_BUCKET: ${GCP_BUCKET}

echo ' '
echo 'objectAdmin'
gsutil iam ch serviceAccount:${DB_SVC_ACCOUNT}:objectAdmin gs://${GCP_BUCKET}
echo gsutil iam ch serviceAccount:${DB_SVC_ACCOUNT}:objectAdmin gs://${GCP_BUCKET}

<!-- 
echo ' '
echo 'legacyBucketOwner'
echo gsutil iam ch serviceAccount:${DB_SVC_ACCOUNT}:legacyBucketOwner gs://${GCP_BUCKET}
gsutil iam ch serviceAccount:${DB_SVC_ACCOUNT}:legacyBucketOwner gs://${GCP_BUCKET}
 -->

```

```

#############
# Take Backup
#
timestamp=`date +%s`
MSG=2nd-DRAFT

echo PROJECT: $GCP_PROJECT
echo INSTANCE: $GCP_INSTANCE
echo DB: $GCP_DB_NAME
echo USER: $GCP_DB_USER
echo BUCKET: $GCP_BUCKET

export BK_PREFIX=${GCP_INSTANCE}
echo BK_PREFIX: ${BK_PREFIX}

export BK_COMMENT='-LIVE-base-INSTALL-0-'
echo COMMENT: $BK_COMMENT

export BK_TIMESTAMP=`date +%s`
echo TIMESTAMP: $BK_TIMESTAMP

#export GCP_FILE=${GCP_DB_NAME}-${BK_COMMENT}-${BK_TIMESTAMP}.gz

export GCP_FILE=${BK_PREFIX}-${BK_COMMENT}-${BK_TIMESTAMP}.gz
echo FILE: ${GCP_FILE}

echo ' '

echo '######################'
echo 'BACKUP DB'
echo BK_PREFIX: ${BK_PREFIX}
echo GCP_INSTANCE: ${GCP_INSTANCE}
echo GCP_BUCKET: ${GCP_BUCKET}
echo GCP_FILE: ${GCP_FILE}
echo GCP_DB_NAME: ${GCP_DB_NAME}
echo ' '

gcloud sql export sql ${GCP_INSTANCE} gs://${GCP_BUCKET}/backups/${GCP_FILE}    \
--database=${GCP_DB_NAME}	 \
--offload


#################
# DOWNLOAD BACKUPls
# cd <BACKUP DIR>

echo GCP_BUCKET: $GCP_BUCKET
echo GCP_FILE: $GCP_FILE
gcloud storage cp gs://${GCP_BUCKET}/backups/${GCP_FILE} .

...

##############
# RE-CREATE DB
# pq: database "photo-app-0-db-0" is being accessed by other users
# CLOSE ALL TABS ACCESSING APP
#
source config/.env-vars
echo ' '
echo 're-create DB'
echo GCP_DB_NAME: $GCP_DB_NAME
echo GCP_INSTANCE: $GCP_INSTANCE
echo ' '
gcloud sql databases delete $GCP_DB_NAME \
    --instance $GCP_INSTANCE

gcloud sql databases create $GCP_DB_NAME \
    --instance $GCP_INSTANCE


################
# RESTORE BACKUP
#
echo ' '
echo GCP_INSTANCE: ${GCP_INSTANCE}
echo GCP_BUCKET: ${GCP_BUCKET}
echo GCP_FILE: ${GCP_FILE}
echo ' '
gcloud sql import sql ${GCP_INSTANCE} gs://${GCP_BUCKET}/backups/${GCP_FILE} \
--database=${GCP_DB_NAME}

```

##################################################################################
##################################################################################

# DOWNLOAD ALL IMAGES
echo GCP_MEDIA_DIR: ${GCP_MEDIA_DIR}
echo " "
gsutil cp -r ${GCP_MEDIA_DIR} .

# UPLOAD ALL IMAGES
cd <BACKUP DIR>
cd images

echo GCP_LOCAL_ICONS: $GCP_LOCAL_ICONS
echo GCP_LOCAL_INAGE: $GCP_LOCAL_INAGE

echo GCP_IMAGE_DIR: $GCP_IMAGE_DIR
echo GCP_ICON_DIR: GCP_ICON_DIR

echo GCP_MEDIA_DIR: ${GCP_MEDIA_DIR}
echo GCP_LOCAL_DIR: ${GCP_LOCAL_DIR}

echo ' '

cd /mnt/c/Users/heidless/Downloads

gsutil -m cp -r \
  "gs://heidless-pfolio-3-bucket/icons" \
  "gs://heidless-pfolio-3-bucket/images" \
  .

gsutil cp -r ${GCP_LOCAL_ICONS} ${GCP_ICON_DIR}

cd ..

cd icons
echo ${GCP_ICON_DIR}
gsutil cp -r * ${GCP_ICON_DIR}
cd ..



##################################################################################
##################################################################################
# local DB backup & install
##################################################################################


<!-- export BK_DB_NAME=rails_pdf_ninja_0_development -->
<!-- export BK_DB_NAME=airbnb_app_1_development -->

export BK_DB_NAME=alpha_blog_development
echo BK_DB_NAME: ${BK_DB_NAME}

export BK_COMMENT='-0-DATA-1-'
echo BK_COMMENT: ${BK_COMMENT}

export BK_TIMESTAMP=`date +%s`
echo TIMESTAMP: ${BK_TIMESTAMP}

export BK_FILE=${BK_DB_NAME}-${BK_COMMENT}-${BK_TIMESTAMP}.pgsql
echo BK_FILE: ${BK_FILE}

echo ' '

# dump local DB
pg_dump -U heidless ${BK_DB_NAME} > ${BK_FILE}



###################################
# reset DB
psql
--
DROP DATABASE airbnb_app_1_development;
DROP DATABASE airbnb_app_1_test;

--

rails db:create

###################################
# import backup
export BK_DB_NAME=${BK_DB_NAME}

export BK_FILE=airbnb_app_1_development--0-PRE-GEO-0--1728568846.pgsql

echo ' '
echo BK_DB_NAME: ${BK_DB_NAME}
echo BK_FILE: ${BK_FILE}
echo ' '

psql -U heidless ${BK_DB_NAME} < ${BK_FILE}

--





