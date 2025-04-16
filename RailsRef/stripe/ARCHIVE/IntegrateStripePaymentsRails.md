
## [Master Stripe Subscriptions with Rails 7: Webhooks & Recurring Payments | Part 2](https://www.youtube.com/watch?v=iTLh9NdN_jM)

---
---
---


## [Integrate Stripe Payments in Rails 7: Complete Guide with Webhooks & ActiveStorage | Part 1](https://www.youtube.com/watch?v=9bGZM4g02Qs)

### create base project
```
############
# config env
#
mkdir rails7stripe
cd !$

echo '3.2.2' > .ruby-version
gem install rails -v 7.2.2.1

gem install htmlbeautifier solargraph

# going to be running 'pg' within docker. so need a local gem version
# /etc/postgresql/14
whereis pg_config     # /usr/bin/pg_config
gem install pg -- --with-pg-config=/usr/bin/pg_config

#########
# new App
#
rails _7.2.2.1_ new rails7stripedemo -j esbuild --css bootstrap --database=postgresql

```

### Gemfile additions
```
vi Gemfile

bundle add debugger --group "development, test"


bundle add webdrivers --group "test"

bundle add dotenv-rails

# manually add folowing
--
group :development do
  ...
  gem 'letter_opener'
  gem "annotate", "~> 3.2"

end
--

```

### devise
- #### [devise-bootstrap5 : docs](https://www.rubydoc.info/gems/devise-bootstrap5)
```
bundle add devise devise-i18n devise-bootstrap5 

rails g devise:install
rails g  devise User role:integer 

vi db/migrations/20250323140514_devise_create_users.rb
--
# change :       t.integer :role
      t.integer :role, default: 0, null: false

--

```

### environment
```
# env
touch .env

vi Dockerfile-postgres
--
FROM postgres:14.6 as baseimage
--

vi docker-compose.yml
--
# connect via psql
# psql -h 192.168.99.100 --port 54320 -U postgres my_database
  # images: "postgres:13.5"

services:
  db:
    build:
      context: .
      dockerfile: Dockerfile-postgres
    container_name: "r7stripe_postgres"
    command: ["postgres", "-c", "logging_collector=on" , "-c", "logging_filename=postgresql.log" , "-c", "logging_statement=all" ]
    environment:
      POSTGRES_PASSWORD: "postgres"
    ports:
      - "54321:5432"      # <DOCKER PORT>:<APP PORT>
    volumes:
      - r7stripe_dbdata:/var/lib/postgresql/data
  redis:
    image: redis:7.0.8
    command: redis-server --apprendonly yes
    volumes:
      - r7stripe_redis_data:/data
    restart: always
    environment: 
      - REDIS_REPLICATION_MODE=master
volumes:
  r7stripe_redis_data:
  r7stripe_dbdata :
  
--

# run container
docker compose up -d

# prep DB
rails db:prepare

# .env
vi .env
--
PORT=3989
DEV_HOST=heidless.ngrok.io

export DBHOST=localhost
export DBPORT=54678
export DBUSER=postgres
export DBPASSWORD=postgres

--

# basic home environment
rails g controller home index

vi config.routes.rb
--
  root "home#index"

--

```

## [ngrok](https://ngrok.com/) : API Gateway
```
# signup/signin with GitHub

account name: rob.lockhart@yahoo.co.uk

# recovery codes
5VZC6VGHQ8
YFTKC8Y9NR
ZYGGXVQYV4
R86UJFN484
CP79G9T94E
YCR2PHEQFS
T23AFUB9CK
5MVU9C8BJP
33WRDVXXTD
727ZC2U3W3

Account Dashboard
->heidless
  -> Getting Started
    -> Setup & Installation
    Linux->Download->x86... <DOWNLOAD>

cp Downloads/ngrok-v3-stable-linux-amd64.tgz  ../ngrok/ngrok-v3-stable-linux-amd64.tgz

cd ../ngrok
sudo tar -xvzf ./ngrok-v3-stable-linux-amd64.tgz -C /usr/local/bin
ngrok config add-authtoken 2uixbRIVJnNvJBwXUM1Eor9zBwU_7GFbHosQRHEEYzhUw6FpR

vi ./ngrok.sh
--
../ngrok --subdomain heidless 3989

--
chmod +x ./ngrok.sh

./ngrok.sh



vi Procfile.dev
--
# change FROM
web: env RUBY_DEBUG_OPEN=true bin/rails server
TO
web: bash -c '. .env && bundle exec rails s -p ${PORT:-3000} -b ${LISTEN_HOST:-localhost}

--

###############
# test ngrok.sh
#



```
