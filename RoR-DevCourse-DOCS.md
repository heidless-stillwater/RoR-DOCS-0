
## resources
- ### [RubyOnRails](https://rubyonrails.org/)
  - ### [RoR-Guides](https://guides.rubyonrails.org/v6.1/)
## AWS
- ## [Deploying a rails application to Elastic Beanstalk](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/ruby-rails-tutorial.html)
- ### Authentication
  - ### [devise](https://github.com/heartcombo/devise)
- ### [replit (github login)](https://replit.com/login)
- ### [rubygems](https://rubygems.org/)
- ### Ruby Style Guides
  - ### [Ruby Style Guide](https://rubystyle.guide/)
  - ### [rspec-style-guide](https://github.com/rubocop/rspec-style-guide)
  - ### [git: ruby-style-guide](https://github.com/rubocop/ruby-style-guide)
- ### [github.io TryRuby](https://try.ruby-lang.org/)


<hr>
<hr>

```
# toggle sidebar visibility
<ctrl>-b

```
<br />
<br />

# [The Complete Ruby on Rails Developer Course](https://www.udemy.com/course/the-complete-ruby-on-rails-developer-course/learn/quiz/4350012#overview)

## rails
```
# bootstrap
rails new test_app_1 -b bootstrap

# tailwind & postgres
rails new test_app_2 --css tailwind --database=postgresql

# controller
rails g controller pages_controller

```

## tailwind
- ### [Install Tailwind CSS with Ruby on Rails](https://tailwindcss.com/docs/installation/framework-guides/ruby-on-rails)
```
rails new test_app_1
cd test_app_1

./bin/bundle add tailwindcss-ruby
./bin/bundle add tailwindcss-rails
./bin/rails tailwindcss:install

cd ./app/assets/stylesheets
touch application.tailwind.css
vi application.tailwind.css
--
@import "tailwindcss";

--

# start build process
./bin/dev

# example tailwind
<h1 class="text-3xl font-bold underline">
  Hello world!
</h1>

bundle exec rake assets:precompile


gem install bundler
bundle update --bundler
bundle lock --add-platform x86_64-linux

```

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
