
# [AWS + Rails](https://www.youtube.com/playlist?list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq)

- ## [route53]()
- ## [2: How to use S3 for Static Site Hosting](https://www.youtube.com/watch?v=eHSN_-4jVl8&list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq&index=2)
  - ## [bootstrap templates](https://startbootstrap.com/)
  - ## create S3 bucket
  ```
  # bucket must be same name as domain name i.e. sourdoughsweetheart.com.
  
  # requires 2 Buckets
  - sourdoughsweetheart.com
  - www.sourdoughsweetheart.com
    - 'www' forwarded to the first

  --
  BUCKET_NAME 1:
  sourdougsweetheart.com

  REGION:
  eu-west-2

  # ACLs Disabled

  # UNBLOCK ALL Public access

  CREATE
  --

  --

  BUCKET_NAME 2:
  www.sourdougsweetheart.com

  REGION:
  eu-west-2

  # ACLs Disabled

  # UNBLOCK ALL Public access

  CREATE

  --

  ```

  - ## upload content to BUCKET NAME 1

  - ## enable BUCKET-NAME-1->properties->'Static website hosting'

  - ## configure permissions 
    - ### BUCKET-NAME-1->Permissions->BlockAllPublicAccess = Off
    - ### BUCKET-NAME-1->Permissions->Policy
      - Edit
      - Policy Examples
        - Granting Read-Only Permission to Anonymous User
        - https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html

        - Copy and Paste this policy:
          - NB: Change Bucket-Name
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::sourdoughsweetheart.com/*"
            ]
        }
    ]
}
```


  - ## view site
    - ## Buckets->sourdoughsweetheart.com->index.html->Object URL
      - ### https://s3.eu-west-2.amazonaws.com/sourdoughsweetheart.com/index.html


- ## [3: How to Create a FREE SSL Certificate with AWS Certificate Manager](https://www.youtube.com/watch?v=RZsdWNucztY&list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq&index=3)


- ## [4: How to Attach an SSL Certificate to an S3 Static Website through CloudFront](https://www.youtube.com/watch?v=pOx9NUJdD_U&list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq&index=4)

- ## [5: How to Create Your First Rails App](https://www.youtube.com/watch?v=ssPPyizWJuc&list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq&index=5)
```

#######################
# init RAILS



/bin/zsh --login
rvm use --default 3.2.2
export PATH="/home/heidless/.rvm/gems/ruby-3.2.2/bin:$PATH"


# ruby version
rvm install 2.7.1 --with-openssl-dir=$HOME/.rvm/usr


/bin/zsh --login

rvm use --default 2.7.1
export PATH="/home/heidless/.rvm/gems/ruby-2.7.1/bin:$PATH"

gem install bundler -v 2.4.22

gem install nokogiri -v 1.15.6
gem install net-imap -v 0.3.7

gem install rails -v 6.1.7.7


#######################
# create APP
rails  _6.1.7.7_ new aws-rails -d postgresql

cd <APP NAME>   # aws-rails

rails db:setup

rails s   # initial RAILS page displayed


#######################
# create landing page

rails g controller pages home

```

- ## [6: How to use AWS SES to send email in a Rails App](https://www.youtube.com/watch?v=YQd2as8HLtU&list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq&index=6)
  - ## [AWS Simple Email Service : SES](https://eu-west-2.console.aws.amazon.com/ses/home?region=eu-west-2#/homepage)
  ```
  VERIFY Domain:
  lockhartarts.co.uk

  <Choose Domain to dedicate to AWS>

  <Perhaps purchase through Route53 to take advantage of the integration benefits... >

  # create SMTP credentials
  AMAZON->SES->Create SMTP Credentials
  --
  IAM username:
  ses-smtp-user-heidless-3

  SMTP user name:
  AKIAYUPGERUWTKECGHGR

  SMTP password:
  BLqpHgEtRs3KMoM4yXujnYKJJC58xfFHj6s4BA1dJ5yz

  
# make sure to delete previous config/credentials.yml.enc
rm config/credentials.yml.enc config/master.key

# password value in file config/credentials.yml.enc
EDITOR='subl --wait' ./bin/rails credentials:edit
--
aws:
  ses_server: email-smtp.eu-west-2.amazonaws.com
  <!-- ses_server: smtp.sendgrid.net' -->
  ses_username: AKIAYUPGERUWTKECGHGR
  ses_password: BLqpHgEtRs3KMoM4yXujnYKJJC58xfFHj6s4BA1dJ5yz
  ses_domain: lockhartarts.co.uk
  ses_default_from: no-reply@lockhartarts.co.uk
  

--












  --

  - # [create SMTP Cedentials]

- ## [7: How to Add Devise Users and Admins to a Rails App](https://www.youtube.com/watch?v=8UIG9Ggu9Q4&list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq&index=7)


- ## [8: How to Create an Amazon EC2 Instance for a Rails App Deployment](https://www.youtube.com/watch?v=M0avxObh8J8&list=PL6lusswsNPHG5csQPFjMExe5XQqr1cJvq&index=8)
  - ## [GIST Instructions](https://gist.github.com/ThomasBush/584dc1e999b34177dd4436c5edb1b24d)
```

--

# generate random passwords

curl 'https://www.random.org/passwords/?num=2&len=15&format=plain&rnd=new'
--
2CPvCcUZwgbYkJa
MrmHcBEU8mzPedY

--



EC2->SecurityGroups->CreateSecurityGroup
--
name:
aws-rails-sg

description:
allow http, https, ssh

inbound rules:
TYPE:   SOURCE:
HTTP    anywhere
HTTPS   anywhere
SSH     my ip

outboud rules:
TYPE:         DESTINATION:
all traffic   anywhere

--

Generate Instance

EC2->LaunchInstance

--
aws-rails-prod-8

Machine image:

Ubuntu 22-04
t2.micro
Storage: 30Gb
Security Group: aws-rails-sg
Keypair: AWS-Keypair-0.pem    # copy to 'config'

--

--

# login to instance
cd config
chmod 400 AWS-Keypair-0.pem
ssh -i "AWS-Keypair-0.pem" ubuntu@ec2-35-176-23-106.eu-west-2.compute.amazonaws.com

--
# create 'deploy' user
sudo adduser deploy
--
2CPvCcUZwgbYkJa

--

# allow sudo for 'deploy'
sudo adduser deploy sudo

--
su deploy
cd ../deploy

ssh-keygen -t rsa -b 4096 -C "rob@test.com"

# copy local public key to authorized-keys
subl ~/.ssh/id_rsa.pub

sudo vi .ssh/authorized-keys

--

# installs 
# Add repositories


curl -sL https://deb.nodesource.com/setup_20.x -o nodesource_setup.sh

curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list

sudo add-apt-repository ppa:redislabs/redis

sudo apt-get update; sudo apt-get install apt-transport-https build-essential ca-certificates curl dirmngr g++ gcc gifsicle git-core gnupg jpegoptim libcurl4-openssl-dev libffi-dev libpq-dev libqt5webkit5-dev libreadline-dev libsqlite3-dev libssl-dev libxml2-dev libxslt1-dev libyaml-dev make nodejs optipng postgresql postgresql-contrib redis-server redis-tools ruby-dev software-properties-common sqlite3 yarn zlib1g-dev -y;

# NB: qt5-default DEPRECATED and so uninstalled/removed from invocation

sudo apt install postgresql postgresql-contrib


# install rvm - for INFO. defaulting to rbenv
#gpg --keyserver keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
#\curl -sSL https://get.rvm.io | bash -s stable
#source /home/deploy/.rvm/scripts/rvm
#rvm install 3.2.2
#rvm --default use 3.2.2

# Install rbenv and ruby-build
[How to install Ruby on Rails with rbenv on Ubuntu 22.04](https://www.swhosting.com/en/comunidad/manual/how-to-install-ruby-on-rails-with-rbenv-on-ubuntu-2204)


sudo apt update
sudo apt upgrade

sudo apt install git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libffi-dev libgdbm-dev libncurses5-dev libsqlite3-dev libtool pkg-config sqlite3 nodejs npm

git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(rbenv init -)"' >> ~/.bashrc
exec $SHELL

git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
echo 'export PATH="$HOME/.rbenv/plugins/ruby-build/bin:$PATH"' >> ~/.bashrc
exec $SHELL

rbenv install 3.2.2

rbenv global 3.2.2

gem install rails

# verify installation
ruby -v
rails -v

# Install Bundler
gem install bundler

# Install Passenger and NGINX 
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 561F9B9CAC40B2F7
sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/apt/passenger focal main > /etc/apt/sources.list.d/passenger.list'
sudo apt-get update
sudo apt-get install -y nginx-extras libnginx-mod-http-passenger
if [ ! -f /etc/nginx/modules-enabled/50-mod-http-passenger.conf ]; then sudo ln -s /usr/share/nginx/modules-available/mod-http-passenger.load /etc/nginx/modules-enabled/50-mod-http-passenger.conf ; fi
sudo ls /etc/nginx/conf.d/mod-http-passenger.conf

# Configure Passenger
sudo vi /etc/nginx/conf.d/mod-http-passenger.conf
replace free ruby version with: passenger_ruby /home/deploy/.rbenv/shims/ruby;

# Configure NGINX
#sudo vi /etc/nginx/sites-enabled/aws-rails.com

sudo vi ec2-35-176-23-106.eu-west-2.compute.amazonaws.com
2CPvCcUZwgbYkJa
--
server {
  listen 80;
  listen [::]:80;

  server_name ec2-35-176-23-106.eu-west-2.compute.amazonaws.com;
  root /home/deploy/aws_rails/current/public;

  passenger_enabled on;
  passenger_app_env production;

  location /cable {
    passenger_app_group_name aws_rails_websocket;
    passenger_force_max_concurrent_requests_per_process 0;
  }

  client_max_body_size 200m;

  location ~ ^/(assets|packs) {
    expires max;
    gzip_static on;
  }
}
--

sudo service nginx start
2CPvCcUZwgbYkJa

sudo su - postgres
--
createuser --pwprompt deploy
MrmHcBEU8mzPedY

createdb -O deploy aws_rails_production

--

# CHEATSHEET
ls /var/log/nginx
2CPvCcUZwgbYkJa

sudo tail /var/log/nginx/access.log
sudo tail -f /var/log/nginx/access.log
sudo vi /var/log/nginx/access.log

sudo tail /var/log/nginx/error.log
sudo tail -f /var/log/nginx/error.log
sudo vi /var/log/nginx/error.log


```

