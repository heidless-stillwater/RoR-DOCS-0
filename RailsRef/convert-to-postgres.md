
# [Switch your Rails app from sqlite to PostgreSQL](https://medium.com/@rossabaker/switch-your-rails-app-from-sqlite-to-postgresql-a0474a29af20)
```
# Gemfile
vi ./Gemfile
```
delete 'sqlite'

# Use postgresql as the database for Active Record
gem 'pg'

```

# config/database.yml
SRC_DIR='../archive/*7/config'
cp ${SRC_DIR}/database.yml ./config/database.yml

vi ./config/database.yml
--
# change db names
​
development:
  <<: *default
  database: YOUR_APP_NAME_HERE_development
​
test:
  <<: *default
  database: YOUR_APP_NAME_HERE_test

# [START cloudrun_rails_database]
production:
  <<: *default
  database: <%= Rails.application.credentials.dig(:gcp, :db_database) %>
  username: <%= Rails.application.credentials.dig(:gcp, :db_username) %>
  password: <%= Rails.application.credentials.dig(:gcp, :db_password) %>
  host: "<%= ENV.fetch("DB_SOCKET_DIR") { '/cloudsql' } %>/<%= Rails.application.credentials.dig(:gcp, :db_instance_host) %>"
# [END cloudrun_rails_database]

--

```

