


# rails
```
gem install rails --version=6.0.2.1
rails _6.0.2.1_ new photo-app

```

# install env versions

- ## list all known ruby versions
```
rvm list known
```

- ## ruby
```
#rvm pkg install openssl

/bin/zsh --login
rvm install 2.6.3 --with-openssl-dir=$HOME/.rvm/usr
export PATH="/home/heidless/.rvm/gems/ruby-2.6.3/bin:$PATH"

bundle install


/bin/zsh --login
rvm use --default 2.6.3
rvm list
```
- ## node
```

nvm use --default 12.22.12
nvm list

npm -v 
--
8.19.2
--

npm install yarn
yarn -v
--
1.22.22
--
```

- ## bundler
```
gem install bundler -v 2.4.22
bundle update --bundler

gem install pg
bundle config set --local without 'production'
bundle install

```

- ## rails
```
gem list rails

gem install rails -v 6.0.1
```
# utils
```

#gem install webpacker # v5.4.4
--
#gem 'webpacker', '~> 5.4', '>= 5.4.4'
--

rm -rf node_modules yarn.lock
yarn install --check-files

--
rails generate controller welcome index

rails g resource UserStock user:references stock:references

rails routes --expanded | grep user_stocks

rails g migration add_first_last_name_to_users

rails g controller friendships create destroy


```

# devise
```
# add to TOP of Gemfile
gem "devise"

rm -rf node_modules Gemfile.lock yarn.lock
yarn install
bundle install --redownload # Forces a redownload of all gems on the gemfile, assigning them to the new bundler
bin/spring stop
rails generate devise:install

```



###############################################
################ history - REFERENCE ######################

nvm use --default 18.12.1
nvm list

rvm use --default 2.5.1
export PATH="/home/heidless/.rvm/gems/ruby-2.5.1/bin:$PATH"
rvm list

rvm use --default 3.1.4
rvm list

nvm use --default 21.7.3
nvm list

gem install rails --version=6.0.2.1
rails _6.0.2.1_ new photo-app

gem install bundler:1.16.5