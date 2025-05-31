

[How to Deploy a Vite App Using GCP](https://medium.com/@DeveloperAdil/how-to-deploy-a-vite-app-using-gcp-2c88d85c9ea2)
```
########
# CREATE
#
npm create vite@latest


```


[Deploy a React Web Application to Google Cloud Platform](https://www.youtube.com/watch?v=GIM5irN4Ix0&t=788s)
```
nvm use 22.12.0
npx create-react-app react-gcp-deploy-test-v1
cd react-gcp-deploy-test-v1 

npm start

# src/App.js
--
# update@
Edit <code>src/App.js</code> and save to reload.

# to:
Project Portfolio Website

--

SRC=/home/heidless/projects/sandbox/RoR/DEANIN-react-on-rails/react-gcp-deploy-test-v1

cp ${SRC}/nginx.conf .
cp ${SRC}/Dockerfile .
cp ${SRC}/.env-vars .
cp ${SRC}/.env.development .


######
# .env
#
cp /home/heidless/projects/heidless-pfolio/pfolio-frontend-0/.env .

# .env
--
BACKEND_HOST=https://h-pfolio-2.nw.r.appspot.com
BACKEND_URL=https://h-pfolio-2.nw.r.appspot.com/
--

############
# .env.development & .env setup
#
npm install dotenv

vi react-on-rails-frontend-v1/.env-development (react-on-rails-frontend-v1/.env)
--

# VITE_POSTS_API_URL=http://localhost:3000/api/v1/posts
# VITE_SEARCH_API_URL=http://localhost:3000/api/v1/search

VITE_POSTS_API_URL=https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app/api/v1/posts
VITE_SEARCH_API_URL=https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app/api/v1/search

--


####################
# Dockerfile
#
# NB: Using port '443'
#
--# Step 1: Build the React app
FROM node:20 as vite-build

WORKDIR /app

# Copy package.json and package-lock.json (or yarn.lock) files
#COPY package*.json yarn.lock ./
COPY package*.json ./


# Install dependencies
RUN yarn

# Copy the rest of your app's source code
COPY . .

# Build your app
RUN yarn build

# Step 2: Serve the app using Nginx
FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/configfile.template

COPY --from=vite-build /app/dist /usr/share/nginx/html

# Expose port 8080 to the Docker host, so we can access it 
# from the outside. This is a placeholder; Cloud Run will provide the PORT environment variable at runtime.
# ENV PORT 8080
ENV PORT=443
ENV HOST=0.0.0.0
ENV API_URL=https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app/api/v1/posts
EXPOSE 8080
CMD sh -c "envsubst '\$PORT' < /etc/nginx/conf.d/configfile.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"

--

# nginx.conf
--
server {
    # listen       ${PORT};
    listen       443;

    server_name  localhost;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
        try_files $uri /index.html;
    }
}

--

# eslint.cjs
--
module.exports = {
  root: true,
  env: {
    browser: true,
    es2020: true,
    // Jest global variables
    jest: true,
    // Node.js global variables and Node.js scoping
    node: true,
  },
  extends: [
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:react/jsx-runtime",
    "plugin:react-hooks/recommended",
    "plugin:jest/recommended",
  ],
  ignorePatterns: ["dist", ".eslintrc.cjs"],
  parserOptions: { ecmaVersion: "latest", sourceType: "module" },
  settings: { react: { version: "18.2" } },
  plugins: ["react-refresh"],
  rules: {
    "react-refresh/only-export-components": [
      "warn",
      { allowConstantExport: true },
    ],
  },
};

--


npm run build

#docker build -t arjuna11/react-on-rails-frontend-v1:latest .

#docker run -p 8080:443 arjuna11/react-on-rails-frontend-v1:latest

## check docker deploy
#http://localhosts:8080
#http://localhosts:8080/api/v1/posts

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

###############################
# Deploy 2 GCP
###############################
#

SRC=/home/heidless/projects/sandbox/nextjs/pfolio-frontend-test-deploy-0

cp ${SRC}/Dockerfile .
cp ${SRC}/.env-vars .
cp ${SRC}/.env .


- copy pfolio_frontend Dockerfile
- copy .env-vars from pfolio_frontend
- replace '.next' with 'dist'
sudo docker build -t react_on_rails_api_v1 .

# Backend
```
# .env
--
## frontend URLs
#BACKEND_HOST=https://h-pfolio-2.nw.r.appspot.com
#BACKEND_URL=https://h-pfolio-2.nw.r.appspot.com/

GCP_BACKEND_HOST=https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app > .env
GCP_BACKEND_URL=https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app/ >> .env

EDITOR='subl --wait' ./bin/rails credentials:edit

=======================================
gcp:
  gcp_api_backend_host: https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app
  gcp_api_backend_url: https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app/

```

# utils - IF NEEDED
```
source ../react-on-rails-api-v1/config/.env-vars
source ./.env-frontend
source ./config/.env-vars 
echo ' '

echo SECRET: ${GCP_SECRET_NAME}
echo ' '
gcloud secrets delete ${GCP_SECRET_NAME}

```

gcloud secrets describe ${GCP_SECRET_NAME}

## create 'secret'
```
source ./config/.env-vars
echo ' '

echo SECRET: ${GCP_SECRET_NAME}
echo ' '
gcloud secrets create ${GCP_SECRET_NAME} --data-file .env

```
--

```

# STORAGE
```
## project
source .env-vars

#################
## storage bucket
#
source .env-vars
echo ' '
echo GCP_BUCKET: ${GCP_BUCKET}
echo GCP_REGION: ${GCP_REGION}

export bucket_path='gs://'${GCP_BUCKET}
echo bucket_path: ${bucket_path}

echo ""
echo "### BUCKET ###"

gsutil mb -l ${GCP_REGION} ${bucket_path}

## [bucket permissions]()
gsutil iam ch allUsers:objectViewer $bucket_path

```

# Cloud Run - SUBMIT
```
## push to repository

source .env-vars

echo gcloud builds submit --tag gcr.io/${GCP_PROJECT}/${GCP_SERVICE_NAME} .

echo ""
echo "### SUBMIT ###"

gcloud builds submit --tag gcr.io/${GCP_PROJECT}/${GCP_SERVICE_NAME} .

```

## build service - DEPLOY
```
GCP_SERVICE_NAME=react-gcp-deploy-test-v2
GCP_REGION=europe-west2

gcloud run services delete ${GCP_SERVICE_NAME} --region ${GCP_REGION}

source .env-vars

echo ""
echo "### DEPLOY ###"

GCP_SERVICE_NAME=react-gcp-deploy-test-v1-svc
GCP_IMAGE_NAME=${GCP_SERVICE_NAME}
# GCP_IMAGE_NAME=react-gcp-deploy-test-v1-svc
GCP_REGION=europe-west2
GCP_PROJECT=h-pfolio-2
GCP_INSTANCE=react-on-rails-api-v1-instance-0
GCP_PORT=443                                

gcloud run deploy ${GCP_SERVICE_NAME} \
     --platform managed \
     --region ${GCP_REGION} \
     --image gcr.io/${GCP_PROJECT}/${GCP_IMAGE_NAME} \
     --add-cloudsql-instances ${GCP_PROJECT}:${GCP_REGION}:${GCP_INSTANCE} \
     --port=${GCP_PORT} \
     --allow-unauthenticated



# URL
https://pfolio-frontend-svc-1089619978780.europe-west2.run.app

```



################################
################################
# RUBY on RAILS
#
# working example
```
#SRC_DIR='/home/heidless/projects/sandbox/RoR/RoR-DevCourse/rails/rails-fin-track-7'
#export SRC_DIR='/home/heidless/projects/heidless-ror/live-apps/saas-project-app-v1'
#export SRC_DIR='/home/heidless/projects/heidless-ror/live-apps/saas-project-app-v1'
#export SRC_DIR='/home/heidless/projects/sandbox/RoR/DEANIN-react-on-rails/react_on_rails_api_v1-TST'

#############
# SRC EXAMPLE
#
export SRC_DIR='/home/heidless/projects/sandbox/RoR/DEANIN-react-on-rails/react_on_rails_api'
echo ${SRC_DIR}

##########
# ENV VARS
#
cp ${SRC_DIR}/config/.env-vars config/.env-vars

vi config/.env-vars
--
export GCP_PROJECT=h-pfolio-2
export GCP_APP_NAME=alpha-blog-act-as-tenant-v1

--
source config/.env-vars

############
# SECRETS
#
cp -rf ${SRC_DIR}/config/secrets config/secrets

vi .gitignore
--
/config/secrets/*

--

##########
# DATABASE
#
cp ${SRC_DIR}/config/database.yml config/database.yml

config/database.yml
-- 
  database: react_on_rails_api_v1_development

  database: react_on_rails_api_v1_test

# [START cloudrun_rails_database]
production:
  <<: *default
  database: <%= Rails.application.credentials.dig(:gcp, :db_database) %>
  username: <%= Rails.application.credentials.dig(:gcp, :db_username) %>
  password: <%= Rails.application.credentials.dig(:gcp, :db_password) %>
  host: "<%= ENV.fetch("DB_SOCKET_DIR") { '/cloudsql' } %>/<%= Rails.application.credentials.dig(:gcp, :db_instance_host) %>"
# [END cloudrun_rails_database]

--  

# cloudbuild.yaml
cp ${SRC_DIR}/cloudbuild.yaml cloudbuild.yaml


# Dockerfile
cp ${SRC_DIR}/Dockerfile Dockerfile

--
FROM ruby:3.2

# FROM ruby:3.2-buster
#ARG RUBY_VERSION=3.2.2
#FROM docker.io/library/ruby:$RUBY_VERSION-buster AS base

--

# Tasks
cp ${SRC_DIR}/lib/tasks/*.rake lib/tasks/



```
