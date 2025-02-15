
# prep environment

## RUBY 3
/bin/zsh --login
export PATH="/home/heidless/.rvm/gems/ruby-3.3.3/bin:$PATH"
rvm use --default 3.3.3

## NODE 20

nvm install 20.15.0
nvm use --default 20.15.0
nvm list

## POSTGRES 14
apt-get update -y

apt install gnupg gnupg2 gnupg1 -y

sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | apt-key add -

apt-get update -y
apt-get install postgresql-14 -y

apt-get -y install postgresql

sudo -u postgres psql -c "SELECT version();"

## REDIS 6.2

### [How to Install Redis on Ubuntu 22.04](https://www.cherryservers.com/blog/install-redis-ubuntu)

```
sudo apt update

sudo apt install redis-server -y

# ESSENTIAL: configure redis
sudo subl /etc/redis/redis.conf
--
replace 'supervised no'
with 'supervised systemd'

--

sudo systemctl restart redis

sudo systemctl status  redis

# start redis on reboot
sudo systemctl enable --now redis-server

redis-server -v

#####################
# test install

redis-cli
--
ping # returns 'PONG'

set city 'London'

get city
--

###########################
# configure authentication
# set 'requirepass <PASSWORD>'
#
sudo subl /etc/redis/redis.conf
--
'requirepass foobared'

--

sudo systemctl restart redis.service

redis-cli
--
get city

# authorise 'default' user with password
AUTH default foobared

--

exit


###########################
# allow remote connections
#
sudo subl /etc/redis/redis.conf
--
# set 'bind'
bind 0.0.0.0

--

sudo systemctl restart redis.service

--

#####################################
# confirm redis remotely accessible
#
sudo ss -an | grep 6379   # should LISTEN on Port 6379

########################
# configure for firewall
#
sudo ufw allow 6379/tcp
sudo ufw reload


```

11:49:57 worker.1           | You are connecting to Redis 6.0.16, Sidekiq requires Redis 6.2.0 or greater


## redis: install 6.2.0

### Download redis-6.2.0.tar.gz from the official http://download.redis.io/releases/ webpage.
```
mkdir redis-install
cd !$
click to download: https://download.redis.io/releases/redis-6.2.0.tar.gz
copy to local redis-install dir

```

### [How To Install and Configure Redis on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-redis-on-ubuntu-16-04)

```
tar xzvf redis-6.2.0.tar.gz
cd redis-6.2.0

# build
make

# test all good?
make test

sudo make install

# configure redis
sudo rm -rf /etc/redis

sudo mkdir /etc/redis

#sudo cp /tmp/redis-stable/redis.conf /etc/redis

sudo cp ./redis.conf /etc/redis


sudo subl /etc/redis/redis.conf
--
#requirepass foobared

supervised systemd

dir /var/lib/redis

--

# create unit file
sudo subl /etc/systemd/system/redis.service
--
[Service]
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf --supervised systemd
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target

--

# create redis user, group & directories
sudo adduser --system --group --no-create-home redis

sudo mkdir /var/lib/redis

sudo chown redis:redis /var/lib/redis

sudo chmod 770 /var/lib/redis

# start the redis service
sudo systemctl daemon-reload
sudo systemctl start redis

# status
sudo systemctl status redis
systemctl status redis-server.service 
journalctl -xeu redis-server.service

```

## yarn
```
# current expected version is 4.2.2
# this fails to install
#yarn set version 4.2.2

# set version in package.json to current version
"packageManager": "yarn@1.22.22"

# refresh yarn
rm yarn.lock
yarn


```





## [Chrome](https://www.google.com/search?q=chrome) (for headless browser tests)

# setup env
```
bundle install

# Gemfile
# Comment out
--
#gem "ruby-vips"


--

##############################
# bin/setup
#
--
# creates database
untitled_application_development
untitled_application_test

--


##############################
# boot server
#
bin/dev


```

# DOCKER install

