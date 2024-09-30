
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

--
ALPHA-BLOG:
serviceAccountEmailAddress: p84348039033-ept8q1@gcp-sa-cloud-sql.iam.gserviceaccount.com

FIN-TRACK:
serviceAccountEmailAddress: p32685880208-q2qf3l@gcp-sa-cloud-sql.iam.gserviceaccount.com

CAT-PHOTO:
serviceAccountEmailAddress: p32685880208-8vpfud@gcp-sa-cloud-sql.iam.gserviceaccount.com

PHOTO-APP:
SQL_SVC_ACC: serviceAccountEmailAddress: p84348039033-dqu1x6@gcp-sa-cloud-sql.iam.gserviceaccount.com

RAILS-V6-1-7-BASE:
SQL_SVC_ACC: serviceAccountEmailAddress: p84348039033-fc24g7@gcp-sa-cloud-sql.iam.gserviceaccount.com

RAILS_TEST_DEPLOY:
SQL_SVC_ACC: serviceAccountEmailAddress: p32685880208-a7kwje@gcp-sa-cloud-sql.iam.gserviceaccount.com

ACTIVE_STORAGE-TST-2:
SQL_SVC_ACC: serviceAccountEmailAddress: p32685880208-tpwqyw@gcp-sa-cloud-sql.iam.gserviceaccount.com

BULLET_TRAIN:
SQL_SVC_ACC: serviceAccountEmailAddress: p84348039033-4sab4e@gcp-sa-cloud-sql.iam.gserviceaccount.com

RAILS_PDF_NINJA:
SQL_SVC_ACC: serviceAccountEmailAddress: p84348039033-ov2wxu@gcp-sa-cloud-sql.iam.gserviceaccount.com
--
#############################
# assing Svc Account
#export DB_SVC_ACCOUNT=backups-svc-account@heidless-ror-5.iam.gserviceaccount.com

# fin-track
export DB_SVC_ACCOUNT=p84348039033-szuu0p@gcp-sa-cloud-sql.iam.gserviceaccount.com

# alpha-blog
export DB_SVC_ACCOUNT=p84348039033-ept8q1@gcp-sa-cloud-sql.iam.gserviceaccount.com

# photo-app
export DB_SVC_ACCOUNT=p84348039033-dqu1x6@gcp-sa-cloud-sql.iam.gserviceaccount.com
echo ${DB_SVC_ACCOUNT}

# cat-photo
export DB_SVC_ACCOUNT=p32685880208-8vpfud@gcp-sa-cloud-sql.iam.gserviceaccount.com

# RAILS-V6-1-7-BASE:
export DB_SVC_ACCOUNT=p84348039033-fc24g7@gcp-sa-cloud-sql.iam.gserviceaccount.com

# RAILS_TEST_DEPLOY:
export DB_SVC_ACCOUNT=p32685880208-a7kwje@gcp-sa-cloud-sql.iam.gserviceaccount.com

# ACTIVE_STORAGE-TST-2:
export DB_SVC_ACCOUNT=p32685880208-tpwqyw@gcp-sa-cloud-sql.iam.gserviceaccount.com

# BULLET_TRAIN:
export DB_SVC_ACCOUNT=p84348039033-4sab4e@gcp-sa-cloud-sql.iam.gserviceaccount.com

# RAILS_PDF_NINJA:
export DB_SVC_ACCOUNT=p84348039033-ov2wxu@gcp-sa-cloud-sql.iam.gserviceaccount.com

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

export BK_PREFIX=${GCP_BACKUP_PREFIX}
echo BK_PREFIX: ${BK_PREFIX}

export BK_COMMENT='-BACKUP-TEST-'
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


###############
# UPLOAD BACKUP
#
source ../../config/.env-vars

GCP_FILE=fin-track-0-instance-0-fin-track-0-db-0-upload-inage-test-0-1719253838.gz

echo GCP_BUCKET: ${GCP_BUCKET}
echo GCP_FILE: ${GCP_FILE}
gcloud storage cp ${GCP_FILE} gs://${GCP_BUCKET}/backups/${GCP_FILE}


##############
# RE-CREATE DB
# pq: database "photo-app-0-db-0" is being accessed by other users
# CLOSE ALL TABS ACCESSING APP
#
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
export BK_DB_NAME=heidless_demo_app_0_development
echo BK_DB_NAME: ${BK_DB_NAME}

export BK_COMMENT='-0-pre-users-0-'
echo BK_COMMENT: ${BK_COMMENT}

export BK_TIMESTAMP=`date +%s`
echo TIMESTAMP: ${BK_TIMESTAMP}

export BK_FILE=${BK_DB_NAME}-${BK_COMMENT}-${BK_TIMESTAMP}.pgsql
echo BK_FILE: ${BK_FILE}

export BK_FILE=${BK_DB_NAME}-${BK_COMMENT}-${BK_TIMESTAMP}.pgsql
echo BK_FILE: ${BK_FILE}

echo ' '

# dump local DB
pg_dump -U heidless ${BK_DB_NAME} > ${BK_FILE}



###################################
# reset DB
psql
--
DROP DATABASE rails_pdf_ninja_0_development;
DROP DATABASE rails_pdf_ninja_0_test;

--

rails db:create

###################################
# import backup
export BK_DB_NAME=rails_pdf_ninja_0_development
<!-- export BK_DB_NAME=active_storage_tst_2_development -->

export BK_FILE=rails_pdf_ninja_0_development--8-PDF-Image-Upload-Local-Storage--1721295085.pgsql

echo ' '
echo BK_DB_NAME: ${BK_DB_NAME}
echo BK_FILE: ${BK_FILE}
echo ' '

psql -U heidless ${BK_DB_NAME} < ${BK_FILE}

--






