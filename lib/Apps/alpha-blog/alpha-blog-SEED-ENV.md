





# airbnb: generate LIVE Sample DATA - test model 'Genre'
```

rails g task bloggers seed_users

rails g task cats seed_categories

rails g task arts seed_articles








#################################################
#################################################

# airbnb: generate LIVE Sample DATA - test model 'Genre'
```
rails g task rentals seed_properties

rails g task logins seed_users




```

rails g model Genre name

rails db:migrate

rails g task movies seed_genres


vi lib/tasks/movies.rake
--
namespace :movies do
  desc "Seeds genres"
  task seed_genres: :environment do
    Genre.create!([{
      name: "Action"
    },
    {
      name: "Sci-Fi"
    },
    {
      name: "Adventure"
    }])

    p "Created #{Genre.count} genres"
  end
end

--

rails -T movies

rails movies:seed_genres

```












## [Split your database seeds.rb by Rails environment](https://railsnotes.xyz/blog/split-seeds-rb-by-rails-environment)

## [seed_migration](https://github.com/pboling/seed_migration)

## [Seeding a Database in Ruby on Rails](https://develclan.com/seeding-database-ruby-on-rails/)
 
## DEV
```
rails new sample; cd sample

bin/rails g model Movie title director storyline:text watched_on:date

bin/rails db:migrate

# ROLLBACK examples
#bin/rails db:rollback
#bin/rails db:rollback STEP=2

####################
# available commands
rails -T

bin/rails -T db

```



## seeds.rb
```
Movie.destroy_all

Movie.create!([{
  title: "Inside Out",
  director: "Pete Docter",
  storyline: "As Riley’s family moves to a new city, her emotions —Joy, Sadness, Anger, Fear, and Disgust— navigate the challenges of adjusting to a new environment.",
  watched_on: 9.years.ago
},
{
  title: "Toy Story 4",
  director: "Josh Cooley",
  storyline: "Woody, Buzz Lightyear and the rest of the gang embark on a road trip with Bonnie and a new toy named Forky. The adventurous journey turns into an unexpected reunion as Woody's slight detour leads him to his long-lost friend Bo Peep.",
  watched_on: 5.years.ago
},
{
  title: "Soul",
  director: "Pete Docter",
  storyline: "After landing the gig of a lifetime, a New York jazz pianist suddenly finds himself trapped in a strange land between Earth and the afterlife.",
  watched_on: 4.years.ago
}])

p "Created #{Movie.count} movies"

--

rails db:seed

# check results
rails runner "p Movie.pluck :title"


```

## Using a custom Rails task to seed actual data
```
rails g model Genre name

rails db:migrate

rails g task movies seed_genres

vi lib/tasks/movies.rake
--
namespace :movies do
  desc "Seeds genres"
  task seed_genres: :environment do
    Genre.create!([{
      name: "Action"
    },
    {
      name: "Sci-Fi"
    },
    {
      name: "Adventure"
    }])

    p "Created #{Genre.count} genres"
  end
end

--

rails -T movies

rails movies:seed_genres

```

## Loading seeds using the console
```
###########
# run console in 'sandbox' mode
#
bin/rails c --sandbox

###########
# rails c

Rails.application.load_seed

```

## Loading more seeds using Faker
```
vi seeds.rb
--
Movie.destroy_all

100.times do |index|
  Movie.create!(title: "Title #{index}",
                director: "Director #{index}",
                storyline: "Storyline #{index}",
                watched_on: index.days.ago)
end

p "Created #{Movie.count} movies"

--

rails db:seed

# sample results
rails runner "p Movie.select(:title, :director, :storyline).last"

```

# sample results
```
rails runner "p Movie.select(:title, :director, :storyline).last"

```

# add 'faker' gem
bundle add faker --group development
```
vi seeds.rb
`
Movie.destroy_all

100.times do |index|
  Movie.create!(title: Faker::Movie.title,
                director: Faker::Name.name,
                storyline: Faker::Lorem.paragraph,
                watched_on: Faker::Time.between(from: 4.months.ago, to: 1.week.ago))
end

p "Created #{Movie.count} movies"

--

rails db:seed

rails runner "p Movie.select(:title, :director, :storyline, :watched_on).last"

```