

# working example
#SRC_DIR='/home/heidless/projects/sandbox/RoR/RoR-DevCourse/rails/rails-fin-track-7'

export SRC_DIR='/home/heidless/projects/heidless-ror/live-apps/photo-app-v3'
echo ${SRC_DIR}

# file checklist
```

#################
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
  database: saas_project_app_v1_development

  database: saas_project_app_v1_test

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

```
