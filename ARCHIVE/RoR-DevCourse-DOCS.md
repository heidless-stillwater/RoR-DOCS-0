
## resources
- ### [RubyOnRails](https://rubyonrails.org/)
  - ### [RoR-Guides](https://guides.rubyonrails.org/v6.1/)
  - ### ActiveRecord
    - ### [Active Record Validations](https://guides.rubyonrails.org/active_record_validations.html)
    - ### [Active Record Associations](https://guides.rubyonrails.org/association_basics.html)
    - ### [Action View Form Helpers](https://guides.rubyonrails.org/form_helpers.html)
  - ### [form helpers](https://guides.rubyonrails.org/form_helpers.html)
  - ### [How to manage multiple Rails versions](https://medium.com/@relativkreativ/how-to-manage-multiple-rails-versions-bf5759d0823b)
  - ### Language References
    - ### [Inject Method: Explained](https://medium.com/@terrancekoar/inject-method-explained-ed531eff9af8)
  - ### UI
    - ### bootstrap
      - ### [Bootstrap themes, templates, and UI tools](https://startbootstrap.com/)
    - ### tailwind
      - ### [Tailwind CSS Alerts - Flowbite](https://flowbite.com/docs/components/alerts/)
      - ### [ReadyMadeUI - tailwind](https://readymadeui.com/)
    - ### [NavBar](https://readymadeui.com/tailwind-components/header)
  - ### [TW Elements open-source UI Kit](https://tw-elements.com/)
    - ### [TW Navbar - Configuration](https://tw-elements.com/docs/standard/navigation/navbar/)
    - ### [TW - javascript](https://tw-elements.com/learn/te-foundations/tailwind-css/javascript/#:~:text=As%20the%20name%20suggests%2C%20Tailwind,offer%20solutions%20related%20to%20it.)
- ### AWS
  - ### [Deploying a rails application to Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ruby-rails-tutorial.html)
- ### Authentication
  - ### [devise](https://github.com/heartcombo/devise)
- ### [will_paginate gem](https://github.com/mislav/will_paginate)
- ### [Rubular: Ruby regular expression editor](https://rubular.com/)
- ### [replit (github login)](https://replit.com/login)
- ### [rubygems](https://rubygems.org/)
- ### [Unsplash: free images](https://unsplash.com/)
- ### Ruby Style Guides
  - ### [Ruby Style Guide](https://rubystyle.guide/)
  - ### [rspec-style-guide](https://github.com/rubocop/rspec-style-guide)
  - ### [git: ruby-style-guide](https://github.com/rubocop/ruby-style-guide)
- ### [github.io TryRuby](https://try.ruby-lang.org/)

<hr>
<hr>


## vsCode
```
# toggle sidebar visibility
<ctrl>-b <ctrl>-b

```
<br />
<br />

# [The Complete Ruby on Rails Developer Course](https://www.udemy.com/course/the-complete-ruby-on-rails-developer-course/learn/quiz/4350012#overview)

## quickref
```
# install rails
gem list rails
gem install rails -v <VERSION>


# list gems
gem list --local  # list all installed rails versions

# create app
rails _7.2.2.1_ new [PROJECT_NAME] -b bootstrap --database=postgresql
rails _7.2.2.1_ new rails-fin-track-7 -b bootstrap --database=postgresql

rails _6.1.7_ new [PROJECT_NAME] --css tailwind --database=postgresql

# list gems installed for current project
bundle list

# gerate 'controller'
rails g controller welcome index 

#######
# RSpec
#
vi Gemfile
--
group :development, :test do
  gem 'rspec-rails', ">= 3.9.0"
end
--
bundle config set path 'vendor/bundle'
bundle install
bin/rails generate rspec:install
#bin/rails webpacker:install 

# list gems installed for current project
bundle list

```


## rails
```

# bootstrap
rails new deploy_test_0 -b bootstrap --database=postgresql

# tailwind & postgres
rails new rails_alpha_blog --css tailwind --database=postgresql
cd rails_alpha_blog
rails db:prepare
rails tailwindcss:build 
rails s

# routes
rails routes --expanded

rails routes | grep <string i.e. model>

# controller
rails g controller pages_controller

vi ./config/routes.rb
--
  # Defines the root path route ("/")
  root "pages#home"

  get "/about", controller: "pages", action: :about

--

# views
<h1 class="text-3xl font-bold underline">
  Hello world!
</h1>

# migration
rails g migration create_articles 
vi create_articles
--
    create_table :articles do |t|
      t.string :title
      t.text :description
      t.timestamps
    end

--
rails db:migrate


```

## console - troubleshoot
```
rails c

article.save    # fails i.e does not satisfy validations
article.errors  # errors object
article.errors.full_messages  

```

## debugger
```
vi *_controller.rb
--
debugger

--
params    #list all received params
params[:id]
params[:controller]
params[:action]

continue

```

## Create Read Update Delete
### create
```
rails c

Article.all

# create new entry - method #1
Article.create(title: "first title", description: "first description")

# create new entry - method #2
article = Article.new
article.title = "second title"
article.description = "second description"
article.save

# create new entry - method #3
article = Article.new(title: "third title", description: "third description")
article.save

--

```
### read | update
```
Article.find(2)

Article.first
Article.last

article = Article.find(2)
article
article.title

articles = Article.all

```

### update
``` 
article.description = "updated second description"
Article.find(2)
article.save
Article.find(2)


```

### delete
```
article = Article.find(2)
article.destroy

Article.find(4).destroy

```

### many to many




# git
```
git status
git add -A
git status
git commit -m "made changes"

```

## tailwind
- ### [Install Tailwind CSS with Ruby on Rails](https://tailwindcss.com/docs/installation/framework-guides/ruby-on-rails)
- ### [resources](https://v3.tailwindcss.com/resources)
  - ### [hero icons](https://heroicons.com/)

- ### [Use Tailwind CSS with Your Rails Forms](https://dev.to/railsdesigner/use-tailwind-css-with-your-rails-forms-4g1b)


## oop
**Encapsulation** - concept of blocking off areas of code and not making it available to the rest of the program

**Abstraction** - is simplifying a complex process of a program, an enterprise software solution for example by modeling classes appropriate for it

**Inheritance** - is used where a class inherits the behavior of another class, referred to as the superclass

**Polymorphism** - is when a class inherits the behaviors of another class, but has the ability to not inherit everything and change some of it’s inherited behaviors. For example to write a method that does something differently from the inherited method

**Classes** - It is a blueprint that describes the state and behavior that the objects of the class all share. A class can be used to create many objects. Objects created at runtime from a class are called instances of that particular class.

## [bcrypt-ruby](https://www.rubydoc.info/gems/bcrypt-ruby#:~:text=bcrypt%2Druby%20automatically%20handles%20the,letters%2C%20that's%20456%2C976%20different%20databases.)
```
gem install bcrypt
gem update --system 3.6.3

```


## Ruby Language Ref
```
# interactice ruby (irb) : ruby shell
irb

# triple equals '==='
# if the value on the right is a member of whatever is on the left

(1..10) === 1   # true
(1..10).include?(1)   # true

SUPPORTS_PATTERN_MATCH_VERSIONS = '2.7.0'..'3.0.0'
SUPPORTS_PATTERN_MATCH_VERSIONS === '2.7.5'   # true

/abc/ === 'abcdef'
# => true

/abc/.match?('abcdef')
# => true

/abc/ === 'abcdef'
# => true

/abc/.match?('abcdef')
# => true

# lambda

add_one_lambda = -> x { x + 1 }

add_one_lambda === 1
# => 2
add_one_lambda.call(1)
# => 2
add_one_lambda.(1)
# => 2
add_one_lambda[1]
# => 2

```

