
# Candidate Tuts
- ## [Using Active Storage in Rails 7](https://pragmaticstudio.com/tutorials/using-active-storage-in-rails)
- ## [UPLOAD IMAGES IN A RAILS APP](https://medium.com/@michaelmunavu83/upload-images-in-a-rails-app-7645b3bf2f76)

# Setup Base App
base app - clone & customize
```
git clone -b BASELINE-APP git@github.com:heidless-stillwater/rails_alpha_blog.git

mv rails_alpha_blog image_upload_v2
cd !$

rails assets:precompile

###############
# GIT intialise
#
rm -rf .git
git init
git add .
git commit -m "initial commit"
#
# REMOTE - via github site.
Repositories->New
https://github.com/heidless-stillwater?tab=repositories->New
--
image-upload-v1
-> .gitignore
-> MIT licence
git@github.com:heidless-stillwater/image-upload-v2.git

--

# PUSH
git remote add origin git@github.com:heidless-stillwater/image-upload-v2.git
git push -f origin main   # NB: '-f' force option. Overwrites remote

######
# GEMS
#
bundle install

##########
# DATABASE
#
vi config/database.yml
--
substitute 'image_upload_v2_development' for 'rails_alpha_blog_development'
--
rails db:prepare

##############
# GCP_APP_NAME
#
vi config/.env-vars
--
export GCP_APP_NAME=images-upload-v1

--
source config/.env-vars

##################
# GIT
# create git 'main
#
git checkout -b main

```
