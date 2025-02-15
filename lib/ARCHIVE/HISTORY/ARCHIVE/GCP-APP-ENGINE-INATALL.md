

# [Run Rails 7 on App Engine flexible environment](https://cloud.google.com/ruby/rails/appengine)

```

gcloud init
--
PROJECT:
heidless-pfolio-deploy-6

--

# create app.yaml
entrypoint: bundle exec rackup --port $PORT
env: flex
runtime: ruby

# generate new secret key
bundle exec rails secret
--
a30c6c3ff5aaae7d2fcba4486bbfcc06c6e525451cb07e58b5baba2b65df9de378e535c01c334ee9f5b0658de2c75a2f21dc7887418ef2fcb4de2daeb51b5bf2
--

# update app.yaml
--
entrypoint: bundle exec rackup --port $PORT
env: flex
runtime: ruby

env_variables:
  SECRET_KEY_BASE: a30c6c3ff5aaae7d2fcba4486bbfcc06c6e525451cb07e58b5baba2b65df9de378e535c01c334ee9f5b0658de2c75a2f21dc7887418ef2fcb4de2daeb51b5bf2
--

```

## Set up an App Engine flexible environment app
```

#gcloud app instances delete

# create app
gcloud app create

# deploy app
gcloud app deploy






```
