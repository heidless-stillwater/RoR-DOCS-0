
# [Dockerize your rails 7 app](https://medium.com/simform-engineering/dockerize-your-rails-7-app-3223cc851129)

# INSTALL

```
sudo chmod 666 /var/run/docker.sock

sudo apt install net-tools
ipconfig /flushdns

#########################
Dockerfile
# ENV RAILS_ENV=production
ENV RAILS_ENV=development

# bundle config set --local without 'development test' && \
bundle config set --local && \

#########################

RAILS_ENV='development'
echo $RAILS_ENV

docker build -f Dockerfile -t rails-0 .

docker run -p 3000:3000 -v $(pwd):/rails rails-0

--

gcloud builds submit --tag gcr.io/heidless-pfolio-deploy-5/alpha-blog-0.

```

# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...
