

## [How to Load Environment Variables from Secret Manager During Build Time in Cloud Build](https://medium.com/@fermey.paul/how-to-load-environment-variables-from-secret-manager-during-build-time-in-cloud-build-540b2bbfaec6)
```


```


## [How to use Secret Manager in Google Cloud Build (GCP)](https://ho3einmolavi.medium.com/how-to-use-secret-manager-in-google-cloud-build-gcp-eb6fad9a2d4a)

### What is Google Cloud Build?
Cloud Build is a service that executes your builds on Google Cloud.
Cloud Build can import source code from various repositories or cloud storage spaces, execute a build to your specifications, and produce artifacts such as Docker containers or Java archives.
Cloud build allows you to run your docker images on the cloud with minimum configuration.

### Setting up the deployment pipeline using Google cloud triggers and Github
```
->CloudBuild->Trigger->CreateTrigger
--
Name:
react-gcp-vite-test-v2-trigger

Region:
europe-west2

--

EVENT->Push To Branch

SOURCE:
heidless-stillwater/react-gcp-vite-test-v2 (Github App)

CONFIGURATION:
- Cloud Build configuration file (yaml or json): 
--
cloudbuild.yaml
--

SERVICE ACCOUNT:
--
669532112637-compute@developer.gserviceaccount.com
--

CREATE

```

############### LOAD SECRETS ###################
[How to Load Environment Variables from Secret Manager During Build Time in Cloud Build](https://medium.com/@fermey.paul/how-to-load-environment-variables-from-secret-manager-during-build-time-in-cloud-build-540b2bbfaec6)
























##########################
# Example NesJS Project
#
```
npm i -g @nestjs/cli
nest new gcp-nesjs-deploy-exemplar-v0
--
npm
--

######
# TEST
#
http://localhost:3000/


############
# Dockerfile
#
--
FROM node:16.14.0-alpine3.14

RUN apk update
RUN apk upgrade

ENV API_ROOT /kittyapp/api

WORKDIR ${API_ROOT}

COPY ./*.json ./

RUN npm install
RUN npm install pm2 -g

COPY ./src ./src

RUN npm run build

EXPOSE 8080

CMD ["pm2-runtime", "dist/main.js"]

--

# cloudbuild.yaml
--
steps:
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/YOUR_PROJECT_ID/kittyapp-io-apis:latest', '-f', 'Dockerfile', '.' ]
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'push', 'gcr.io/YOUR_PROJECT_ID/kittyapp-io-apis:latest' ]
- name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
  entrypoint: bash
  args: ['-c', 'gcloud run deploy kittyapp-io-apis --image gcr.io/YOUR_PROJECT_ID/kittyapp-io-apis:latest --region europe-west2 --cpu 2 --memory 512Mi --port 8080 --allow-unauthenticated --platform managed']
images:
- 'gcr.io/YOUR_PROJECT_ID/kittyapp-io-apis:latest'
timeout: 3600s

--


#################
# Build/Run Local
#
# local
http://localhost:3000

# docker
docker build -t gcp-nesjs-deploy-exemplar-v0 .
docker run --rm -p 3002:3002 gcp-nesjs-deploy-exemplar-v0

http://localhost:3002

# GCP








```