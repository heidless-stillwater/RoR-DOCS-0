

# [Upload Different File Types With Active Storage In Ruby On Rails 7](https://www.youtube.com/watch?v=B0mvdMtYrm8)


# [Mastering File Upload Using Ruby on Rails Active Storage](https://www.bacancytechnology.com/blog/using-rails-active-storage)
```
gem install rails -v 6.1.7.7

/bin/zsh --login
export PATH="/home/heidless/.rvm/gems/ruby-3.2.2/bin:$PATH"
rvm use --default 3.2.2

rails  _6.1.7.7_ new active-storage-tst-2 -d postgresql

cd <APP NAME>

controllers/welcome_controller.rb
--
class WelcomeController < ApplicationController
  #skip_before_action :authenticate_user!, only: [:index]

  def index
  end
end

--

views/welcome/index.html.erb
--
<h2>Welcome to My App!</h2>
<p class="lead">
  I hope you find it useful...
</p>

--

config/routes.rb
--
root to: "welcome#index"

--

rails db:create
rails db:migrate

```

cat /dev/urandom | LC_ALL=C tr -dc '[:alpha:]'| fold -w 50 | head -n1 > dbpassword

<GOOGLE Deploy files>
--
cp ../active-storage-tst-0/Dockerfile .
cp ../active-storage-tst-0/cloudbuild.yaml .

--

config/.env-vars
--
export GCP_APP_NAME=<APP NAME>

--
source config/.env-vars


Gemfile
--
ruby '3.2.2'

gem "dotenv-rails", "~> 3.1.2"

gem 'base64'
gem 'bigdecimal'
gem 'mutex_m'

group :production do
  gem 'rails_12factor'  
end

--
npm install @babel/plugin-proposal-private-methods
npm install @babel/plugin-proposal-private-property-in-object
npm install @babel/plugin-transform-private-methods
npm install fsevents --save-optional

rm package.lock.json

rails webpacker:install

config/database.yml
--
# [START cloudrun_rails_database]
production:
  <<: *default
  database: <%= ENV["PRODUCTION_DB_NAME"] %>
  username: <%= ENV["PRODUCTION_DB_USERNAME"] %>
  password: <%= Rails.application.credentials.gcp[:db_password] %>
  host: "<%= ENV.fetch("DB_SOCKET_DIR") { '/cloudsql' } %>/<%= ENV["CLOUD_SQL_CONNECTION_NAME"] %>"
# [END cloudrun_rails_database]

--

gcloud init
--
<PROJECT INFO>

--

########################################################
# Step-by-Step Guide to Setting Up Active Storage Rails

bin/rails active_storage:install
bin/rails db:migrate




# [Active Storage](https://guides.rubyonrails.org/active_storage_overview.html)


# [Uploading a PDF to s3 in Rails](https://medium.com/@mattrice12/uploading-a-pdf-to-s3-in-rails-b963a2e53feb)

# [Generating PDF Reports within Rails using Prawn PDF and Prawn Table Gems](https://nicholusmuwonge.medium.com/generating-pdf-reports-within-rails-using-prawn-pdf-and-prawn-table-gem-c5fea5b73e7f)


# [Prawn VS Wicked PDF](https://darktef.github.io/posts/2017-12-prawn-vs-wickedpdf)
- ## [Prawn by example - MANUAL](https://prawnpdf.org/manual.pdf)
- ## [GIT - manual](https://github.com/prawnpdf/prawn/tree/master/manual)


# [Split PDF File into Individual Pages in Ruby](https://docs.aspose.com/pdf/java/split-pdf-file-into-individual-pages-in-ruby/)

# [Working with PDFs in Ruby](https://www.honeybadger.io/blog/ruby-pdfs/)
- ## [wicked_pdf](https://github.com/mileszs/wicked_pdf)
- ## [Prawn PDF](https://github.com/prawnpdf/prawn)
- ## [pdfkit](https://github.com/pdfkit/pdfkit)


# [Wicked PDF vs Puppeteer - Creating PDFs in Rails 6.0](https://www.youtube.com/watch?v=DwBwy2itv1Y)





