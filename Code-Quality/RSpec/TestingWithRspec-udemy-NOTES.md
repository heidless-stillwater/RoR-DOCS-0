# Resources
- [Understanding Ruby Series' Articles](https://dev.to/baweaver/series/11177)
  - [Understanding Ruby - Triple Equals](https://dev.to/baweaver/understanding-ruby-triple-equals-2p9c)

---

---
# Course
## [usemy: testing ruby with RSpec](https://www.udemy.com/course/testing-ruby-with-rspec/learn/lecture/12409324#overview)

### initialize app
```
#RAILS_BUILD_VERSION=6.1.7.7
#gem install rails -v ${RAILS_BUILD_VERSION}
#export PATH="/home/heidless/.rvm/gems/ruby-3.2.2/bin:$PATH"
#/bin/zsh --login

rvm use --default 3.2.2

# new app: '-t' = 'skip test files = '--skip-test'

RAILS_BUILD_VERSION=7.2.2.1
APP_NAME=udemy-RSTestingRuby-v2
rails _${RAILS_BUILD_VERSION}_ new ${APP_NAME} --css bootstrap -d postgresql -t


# initialize rspec
rspec --init

```

---
## TESTS Reviewed
Reseached tests in **./rspec-course-incomplete/spec**
#


## Example Model - card.rb
```
# card_spec.rb
--
RSpec.describe 'Card' do
  it 'has a type' do
    card = Card.new("Ace of Spades")
    expect(card.type).to equal("Ace of Spades")
  end
end
--

# card.rb
--
class Card
  attr_reader :type

  def initialize(type)
    @type = type
  end
end

# rspec card_spec.rb --format progress

rspec card_spec.rb --format documentation

```


