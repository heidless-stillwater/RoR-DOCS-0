
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
#
cd BACKUPS
source ../config/.env-vars
#
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
source ../../config/.env-vars
#
timestamp=`date +%s`

export BK_COMMENT='-LTS-ALPHA-1-0-'
echo COMMENT: $BK_COMMENT

MSG=INITIAL
echo PROJECT: $GCP_PROJECT
echo INSTANCE: $GCP_INSTANCE
echo DB: $GCP_DB_NAME
echo USER: $GCP_DB_USER
echo BUCKET: $GCP_BUCKET

export BK_PREFIX=${GCP_INSTANCE}
echo BK_PREFIX: ${BK_PREFIX}

export BK_TIMESTAMP=`date +%s`
echo TIMESTAMP: $BK_TIMESTAMP

#export GCP_FILE=${GCP_DB_NAME}-${BK_COMMENT}-${BK_TIMESTAMP}.gz

export GCP_FILE=${BK_PREFIX}-${BK_COMMENT}-${BK_TIMESTAMP}.gz
echo FILE: ${GCP_FILE}

echo ' '

echo '######################'
echo 'BACKUP DB'
echo BK_PREFIX: ${BK_PREFIX}
echo GCP_DB_NAME: ${GCP_DB_NAME}
echo GCP_INSTANCE: ${GCP_INSTANCE}
echo GCP_BUCKET: ${GCP_BUCKET}
echo GCP_FILE: ${GCP_FILE}
echo ' '

gcloud sql export sql ${GCP_INSTANCE} gs://${GCP_BUCKET}/backups/${GCP_FILE}    \
--database=${GCP_DB_NAME}	 \
--offload

#################
# DOWNLOAD BACKUPl
# cd <BACKUP DIR>
#export GCP_FILE=h-image-upload-v2-instance-0--BASELINE-DEPLOY-0--1743416815.gz

echo GCP_BUCKET: $GCP_BUCKET
echo GCP_FILE: $GCP_FILE
gcloud storage cp gs://${GCP_BUCKET}/backups/${GCP_FILE} .




###############
# UPLOAD BACKUP
# cd <BACKUP DIR>
#
#echo GCP_FILE: rails-photo-app-v2-instance-0--BASELINE-1--1743601938.gz
#
echo GCP_BUCKET: $GCP_BUCKET
echo GCP_FILE: $GCP_FILE
gcloud storage cp ./${GCP_FILE} gs://${GCP_BUCKET}/backups 

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
# h-pfolio-2-rails-fin-trac-v1-bucket-0/backups/rails-fin-trac-v1-instance-0--LIVE-CONFIG-0--1742463087.gz

# GCP_FILE: rails-fin-trac-v1-instance-0--LIVE-DEPLOY-1--1742759385.gz

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

```

################
# BACKUP - local
#
source ../../config/.env-vars
#
#export BK_DB_NAME=rails_fin_trac_7_development
#export BK_DB_NAME=rails_alpha_blog_development
#export BK_DB_NAME=photo_app_v1_development
#export BK_DB_NAME=rails_alpha_blog_ActAsTenant_develpment

export BK_DB_NAME=saas_project_app_v1_development

echo BK_DB_NAME: ${BK_DB_NAME}

export BK_COMMENT='-local-LTS-ALPHA-1-0-'
echo BK_COMMENT: ${BK_COMMENT}

export BK_TIMESTAMP=`date +%s`
echo TIMESTAMP: ${BK_TIMESTAMP}

export BK_FILE=${BK_DB_NAME}-${BK_COMMENT}-${BK_TIMESTAMP}.pgsql
echo BK_FILE: ${BK_FILE}

echo ' '

###############
# DUMP local DB
#
pg_dump -U heidless ${BK_DB_NAME} > ${BK_FILE}
gzip ${BK_FILE}





############
# COMPRESS
#
filename=$(basename -- "$BK_FILE")
echo $filename

BASENAME="${filename%.*}"
echo $BASENAME

#extension="${filename##*.}"
#echo $extension


# RENAME
# sudo apt-get install rename
#mv ${filename} ${BASENAME}
rename 's/.psql$//' *

# filename root i.e. without suffix
export FILE=${BASENAME}
echo ${FILE}

gzip ${FILE}

### UNCOMPRESS - RESTORE
#
gunzip ${FILE}.gz
mv ${FILE} ${FILE}.psql


##########
# reset DB
#export BK_DB_NAME=saas_project_app_v1_development

psql
--
DROP DATABASE saas_project_app_v1_development;

DROP DATABASE <TEST DB>;

--
rails db:prepare

####################################
# generate SEED data
#
bundle exec rails organizations:seed_organizations


###################################
# import backup
export BK_DB_NAME=${BK_DB_NAME}

export BK_FILE=saas_project_app_v1_development--local-BASELINE-2--1744449857.pgsql
echo BK_FILE:${BK_FILE}

gunzip ${BK_FILE}.gz


echo ' '
echo BK_DB_NAME: ${BK_DB_NAME}
echo BK_FILE: ${BK_FILE}
echo ' '

psql -U heidless ${BK_DB_NAME} < ${BK_FILE}

--


```


