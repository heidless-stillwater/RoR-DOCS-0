
zellij setup --dump-layout default > ~/zellij-layouts/my-quickstart-layout-file.kdl
zellij --layout ${HOME}/zellij-layouts/split-pane-1.kdl

### [Seed data with image attachments via ActiveStorage](https://medium.com/@andrewasmit/seed-data-with-image-attachments-via-activestorage-45c8516c19ea)

### Testing Tuts
- # STUDY THESE!
  - ### [WDS:Introduction To Testing In JavaScript With Jest](https://www.youtube.com/watch?v=FgnxcUQ5vho)
  - ### [React JS Jest: Beginner's Guide](https://daily.dev/blog/react-js-jest-beginners-guide)

- ### [JavaScript Tutorials](https://www.youtube.com/playlist?list=PLTjRvDozrdlxEIuOBZkMAK5uiqp8rHUax)
- ### [JavaScript Unit Testing Tutorial for Beginners](https://www.youtube.com/watch?v=zuKbR4Q428o)
- ### [React Testing for Beginners: Start Here!](https://www.youtube.com/watch?v=8Xwq35cPwYg)

## [JavaScript Tutorials](https://www.youtube.com/playlist?list=PLTjRvDozrdlxEIuOBZkMAK5uiqp8rHUax)
- ### [Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
- ### [Difference Between Promise and Async/Await](https://medium.com/version-1/difference-between-promise-and-async-await-95e453182f55)

## [JavaScript Unit Testing Tutorial for Beginners](https://www.youtube.com/watch?v=zuKbR4Q428o)
```


```
#############################
## [React JS Jest: Beginner's Guide](https://daily.dev/blog/react-js-jest-beginners-guide)
#
```


```

##############
# JEST Testing
npm run test -- --passWithNoTests
- ### Install/Config
  - ### [jest config:](https://www.youtube.com/watch?v=BrGDg7JLspA&list=PL3mtAHT_eRewtt6HPMHFB4TMxkxiEfp9N&index=10)
- ### [Jest Testing like a Pro - Tips and tricks](https://dev.to/dvddpl/jest-testing-like-a-pro-tips-and-tricks-4o6f)
- ### Tuts
  - ### [React Testing for Beginners: Start Here!](https://www.youtube.com/watch?v=8Xwq35cPwYg)

```
# install JEST
npm install --save-dev jest @testing-library/jest-dom @testing-library/react @testing-library/user-event babel-jest @babel/preset-env @babel/preset-react vite-plugin-testing babel-plugin-transform-import-meta jest-environment-jsdom eslint-plugin-jest jest-fetch-mock history

npm install jest-fixed-jsdom --save-dev
nvm install 20.5.1
nvm use 20.5.1

# .babelrc
--
{
  "presets": ["@babel/preset-env", 
  ["@babel/preset-react",]],
  "plugins": [ ["babel-plugin-transform-import-meta", { "module": "ES6" }]]
}


# .eslintrc.cjs
--
...
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
...
--

# jest.config.cjs
module.exports = {
  transform: {
    "^.+\\.jsx?$": "babel-jest",
  },
  setupFilesAfterEnv: ["@testing-library/jest-dom"],
  testEnvironment: "jsdom",
  // testEnvironment: "jest-fixed-jsdom",
  moduleNameMapper: {
    "\\.(css|less|sass|scss)$": "<rootDir>/__mocks__/styleMock.js",
  },
};
--

# mocks
mkdir __mocks__
cd !$

# ./styleMocks.js
--
// __mocks__/styleMocks.js
module.exports = {};
--

# ./constants.js
// __mocks__/constants.js
export const API_URL = 'http://mocked-api-url';

--

###############
# Code Coverage
Displays the coverage of the tests

# package.json
--
...
  "scripts": {
...
    "test": "jest --passWithNoTests",
    "coverage":"jest --coverage",
...
  },
...
--

# .gitignore
...


...

```


############
# TypeScript
- ### [The Ultimate TypeScript Course](https://codewithmosh.com/p/the-ultimate-typescript)
#


###############################################
# [React on Rails](https://www.youtube.com/playlist?list=PL3mtAHT_eRewtt6HPMHFB4TMxkxiEfp9N)

## DoorKeeper
### [API User Accounts For React | React On Rails 7 WishList Part 1](https://www.youtube.com/watch?app=desktop&v=KnTtdxRlEE0)

#################
# INITIALIZE
#
```
rails _7.2.2.1_ new react_on_rails_api_v1  --api --database=postgresql

rails _7.2.2.1_ new rails-push-test_v1  --api --database=postgresql

cd react_on_rails_api_v1

rails db:prepare
```

###################
# RSPEC : Attempt 2
#
```
######
# Gems
#
# RSpec Rails
# https://github.com/rspec/rspec-rails

bundle add rspec-rails --group "development, test"
bundle add factory_bot_rails --group "development, test"
bundle add faker
#
bundle add shoulda-matchers --group "development, test"

rails g rspec:install

# spec/rails_helper.rb
--
...
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
--
bundle config set path 'vendor/bundle'
bundle install

#rails generate rspec:model post
#rails generate rspec:controller post

```

###############
# POST Model
#
```
rails g scaffold Post title:string body:text
rails db:migrate


# config/routes.rb
--
  root "posts#index"

--

```

#########
# GEMS
#
```
# Gemfile
--
# Use Rack CORS for handling Cross-Origin Resource Sharing (CORS), making cross-origin Ajax possible
gem "rack-cors"
--

bundle install


# config/initializers/cors.rb
--
# config as follows
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    # origins "example.com"    
    
    origins "http://127.0.0.1:5173"   # react app's IP address

    # origins "*"   # anyone

    resource "*",
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end

--

```

###########
# SEED Data
#
```
# db/seed.rb
--

Post.destroy_all 

# create 20 posts
20.times do
  Post.create(
    title: Faker::Lorem.sentence(word_count: 3),
    body: Faker::Lorem.paragraph(sentence_count: 3)
  )
end
--
rails db:seed

```

# CheatSheet
```
rspec --format documentation

rspec spec/models/post_spec.rb --format progress
rspec spec/models --format progress

rspec spec/models --format documentation

```

#############
# APP - contd
#
```
rails generate model User password:string email:string
rails db:migrate db:test:prepare

rails generate migration AddUserToPosts user_id:integer 
rake db:migrate db:test:prepare

```

#####################
# Add IMAGES to Posts
#
- Reference Project: [Upload Multiple Images To Active Storage API! | Ruby On Rails 7 Tutorial](https://www.youtube.com/watch?v=U1IwoFo5pJA)
```
rails active_storage:install
rails db:migrate

# app/post/models/post.rb
--

--



```


#########################################################################################################
                                  ############ FRONTEND #############
#########################################################################################################

########
# GITHUB
#
- [react_on_rails_api_v1](https://github.com/heidless-stillwater/react_on_rails_api_v1)
```
# rails and ISSUE
Issues->NewIssue
--
Setup VITE React App
- [] Create VITE App
- [] Refact API Routes to use /api/v1 routing type stuff

--
->Create
->Create a branch->Checkout locally
--
git fetch origin
git checkout 4-setup-vite
--

```

##############
# ROUTES
#
```
Rails.application.routes.draw do
  # API Rputes should be in '/api/v1'
  namespace :api do 
    namespace :v1 do 
      resources :posts
    end
  end

```

#####################
# CONTROLLER
#
```
# move posts_controller.rb
DIR=app/controllers/api/v1
mv posts_controller.rb $DIR

# posts_controller.rb
--
# class PostsController < ApplicationController
# becomes

class Api::v1::PostsController < ApplicationController
--

```

#################################
#################################
# RUN - Rails App
```
# local
rails s
http://127.0.0.1:3000

```



#################################
#################################
# FRONTEND : VITE App
#################################
#
```
npm create vite@latest
--
react-gcp-vite-test-v2
react 
javascript
--

cd react-gcp-vite-test-v2
npm install
npm run dev


############
# .env setup
#
npm install dotenv

vi client/.env-development
--
#VITE_API_URL=http://localhost:3000/api/v1/posts
VITE_API_URL=https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app

--

vi client/.gitignore
--
# DONT PUSH ENV FILES
.env*

--

```

##########################
# SECRETS
#
```
<!-- EDITOR='subl --wait' ./bin/rails credentials:edit -->

# NB: Production Specific?
EDITOR='subl --wait' bin/rails credentials:edit --environment production

gcp:
  db_password: kiCuPptHyyQujIQHEdpIGDirwnvbzswXjIfPHgXmQrRaMSnKqr
  db_database: react-on-rails-api-v1-db-0
  db_username: react-on-rails-api-v1-user-0
  db_instance_host: h-pfolio-2:europe-west1:react-on-rails-api-v1-instance-0
gcs_storage:
  gcs_bucket: h-pfolio-2-react-on-rails-api-v1-bucket-0
  gcs_keyfile: config/secrets/gcs.keyfile
  gcs_storage_prefix: react-on-rails-api-v1
yahoo_finance_api:
  api_key: e7f6e7637fmshab60958cd514432p13a37fjsn26bb0feacaee
finnhub_api:
  api_key: cvamkq1r01qsapma7mj0cvamkq1r01qsapma7mjg
stripe:
  test_publishable_key: pk_test_51PLjRQGNOeNKBTc3wA60vBCW1bdhLVc4Y8wP4NX5E1cXiAh2TNYYgGThPvkcEYakAoOS6VNjFlKEBeweZsRnfQ1E009yKKEh2B
  test_secret_key: sk_test_51PLjRQGNOeNKBTc3eZKbQPlaUWyanXsmwNlHyHkiCUL0AmaG69zx6h2TOOxvXHm6XphlGbQTgr2ML9gzyEmj9mS600VMH0777F
  webhook_secret: whsec_6856dc192142555126bb021b6a9e4ea56bca247adb76a71438d4bd6d4f55c6c4
sendgrid_mailer:
  user_name: apikey
  api_key_secret: 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
  mail_sender: lockhart.r@gmail.com
  domain: naught4profit.co.uk
```

##################
# Draft GCP Deploy
- [How to Deploy a Vite App Using GCP](https://medium.com/@DeveloperAdil/how-to-deploy-a-vite-app-using-gcp-2c88d85c9ea2)
```

##################
# CONFIGURE
#
# src/constants.js
```
// export const API_URL = 'https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app/api/v1/posts';

export const API_URL = 'http://localhost:3000/api/v1/posts';

```

###########################
# Dockerfile - ./Dockerfile
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
ENV PORT 8080
ENV HOST 0.0.0.0
EXPOSE 8080
CMD sh -c "envsubst '\$PORT' < /etc/nginx/conf.d/configfile.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"

--

####################
# NGINX - nginx.conf
#
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


###########
# BUILD/RUN
#

# local
npm run dev

# docker
#npm run build   # generate 'dist/'

docker build -t arjuna11/react-gcp-vite-test-v2:latest .
docker run -p 8080:443 arjuna11/react-gcp-vite-test-v2:latest

# check local deploy
http://127.0.0.1:8080

# FROM node:22.12.0-alpine as build

``

##############################
# GCP: SUBMIT & DEPLOY SERVICE
#

GCP_REGION=europe-west2
GCP_SERVICE_NAME=react-gcp-vite-test-v2

GCP_REGION=us-central1

GCP_REGION=europe-west1
GCP_SERVICE_NAME=react-gcp-vite-test-v2-svc

gcloud run services delete ${GCP_SERVICE_NAME} --region ${GCP_REGION}

Git Repository:
git@github.com:heidless-stillwater/react-gcp-vite-test-v2.git

```

#############################
# MANUAL OPTION MOST RELIABLE
```
## set 'Environment variables'
#--
#VITE_API_URL=https://react-on-rails-api-v1-svc-669532112637.europe-west1.run.app/api/v1/posts
#--

```

###############################
# GCP Set Environment Variables
###############################
- [Configure environment variables for services](https://cloud.google.com/run/docs/configuring/services/environment-variables)
```

################
# Deploy SERVICE
#
gcloud run deploy SERVICE --image IMAGE_URL
   --set-env-vars "KEY1=VALUE1" \
   --set-env-vars "KEY2=VALUE2" \
   --set-env-vars "KEY3=VALUE3"

##############
# set DEFAULTS
#
# Dockerfile
--
ENV KEY1=VALUE1,KEY2=VALUE2
--

#####################################
# VIEW environment variables settings
#
gcloud run services describe SERVICE

############################
# Delete environment variables
#
gcloud run services update SERVICE --remove-env-vars KEY1,KEY2

```

#############################
# SUBMIT to Artifact Registry
#
```
echo ' '
echo GCP_REGION: ${GCP_REGION}
echo GCP_PROJECT_ID: ${GCP_PROJECT_ID}
echo GCP_REPOSITORY_NAME: ${GCP_REPOSITORY_NAME}
echo GCP_IMAGE_NAME: ${GCP_IMAGE_NAME}

echo ' '
echo "### SUBMIT app TO REGISTRY ###"
echo ' '

echo "gcloud builds submit --config cloudbuild.yaml --substitutions _GCP_REGION=${GCP_REGION} --substitutions _GCP_REPOSITORY_NAME=${GCP_REPOSITORY_NAME} --substitutions _GCP_IMAGE_NAME=${GCP_IMAGE_NAME}"

gcloud builds submit --config cloudbuild.yaml \
  --substitutions _GCP_REGION=${GCP_REGION} \
  --substitutions _GCP_REPOSITORY_NAME=${GCP_REPOSITORY_NAME} \
  --substitutions _GCP_PROJECT_ID=${GCP_PROJECT_ID} \
  --substitutions _GCP_IMAGE_NAME=${GCP_IMAGE_NAME}

gcloud builds submit --config cloudbuild.yaml --substitutions _GCP_REGION=${GCP_REGION} --substitutions _GCP_REPOSITORY_NAME=${GCP_REPOSITORY_NAME} --substitutions _GCP_PROJECT_ID=${GCP_PROJECT_ID} --substitutions _GCP_IMAGE_NAME=${GCP_IMAGE_NAME}



```

#####################
# DEPLOY to Cloud Run
#
```
source ./config/.env-vars
echo ' '

echo GCP_SERVICE_NAME: ${GCP_SERVICE_NAME}
echo GCP_REGION: ${GCP_REGION}
echo GCP_PROJECT: ${GCP_PROJECT}
echo GCP_INSTANCE: ${GCP_INSTANCE}
echo GCP_SECRET_NAME: ${GCP_SECRET_NAME}

echo ' '
echo "### DEPLOY app TO CLOUD RUN ###"
echo ' '
echo "
gcloud run deploy ${GCP_SERVICE_NAME} \
     --platform managed \
     --region ${GCP_REGION} \
     --image gcr.io/${GCP_PROJECT}/${GCP_SERVICE_NAME} \
     --add-cloudsql-instances ${GCP_PROJECT}:${GCP_REGION}:${GCP_INSTANCE} \
     --allow-unauthenticated
"

gcloud run deploy ${GCP_SERVICE_NAME} \
     --platform managed \
     --region ${GCP_REGION} \
     --image gcr.io/${GCP_PROJECT}/${GCP_SERVICE_NAME} \
     --add-cloudsql-instances ${GCP_PROJECT}:${GCP_REGION}:${GCP_INSTANCE} \
     --allow-unauthenticated

```













