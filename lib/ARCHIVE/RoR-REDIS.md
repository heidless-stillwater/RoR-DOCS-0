



# [Create and manage Redis instances](https://cloud.google.com/memorystore/docs/redis/create-manage-instances)
## [MemoryStore](https://console.cloud.google.com/memorystore/redis/instances?project=heidless-ror-6)
```

export REDIS_INSTANCE_NAME=heidless-redis-instance-0
echo REDIS_INSTANCE_NAME: ${REDIS_INSTANCE_NAME}

export REDIS_SIZE=5
echo REDIS_SIZE: ${REDIS_SIZE}

export REDIS_REGION=europe-west1
echo REDIS_REGION: ${REDIS_REGION}

export REDIS_DISPLAY_NAME=heidless-display-name
echo REDIS_DISPLAY_NAME: ${REDIS_DISPLAY_NAME}



====================================================================

# create
gcloud redis instances create ${REDIS_INSTANCE_NAME} --size=${REDIS_SIZE} --region=${REDIS_REGION}
--
# edit cable.yml with redis url
# i.e. 10.10.115.203:6379

--

# list
gcloud redis instances list --region=europe-west1

# update
gcloud redis instances update heidless-redis-instance-0 --region=europe-west1 --display-name=${REDIS_DISPLAY_NAME}

# delete
gcloud redis instances delete heidless-redis-instance-0 --region=europe-west1


```

# [Connecting to a Redis instance from a Cloud Run service](https://cloud.google.com/memorystore/docs/redis/connect-redis-instance-cloud-run)

```
gcloud redis instances describe ${REDIS_INSTANCE_NAME} --region ${REDIS_REGION} --format "value(authorizedNetwork)"


```

### [Sample Application](https://cloud.google.com/memorystore/docs/redis/connect-redis-instance-cloud-run)
