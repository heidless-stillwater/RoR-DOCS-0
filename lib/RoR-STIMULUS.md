
## [STIMULUS Reference]()
- ## [Stimulus-Hotwired Lifecycle Callbacks](https://stimulus.hotwired.dev/reference/lifecycle-callbacks)
- ## [Stimulus-Hotwired Actions](https://stimulus.hotwired.dev/reference/actions)
- ## [Stimulus-Hotwired Targets](https://stimulus.hotwired.dev/reference/targets)

## [Rails Generate controller: Reference & Command Builder](https://railsg.xyz/controller)

## [Procfile.dev, bin/dev, and Rails 7 — how they work, why they're great](https://railsnotes.xyz/blog/procfile-bin-dev-rails7)

## [Your first Stimulus Controller — Learn Stimulus in Ruby on Rails by building a toggle](https://railsnotes.xyz/blog/your-first-stimulus-controller-learn-stimulus-ruby-on-rails-by-building-a-toggle-beginners-guide)
```
rails new first_stimulus_controller_toggle --css tailwind && cd first_stimulus_controller_toggle

rails g controller home index

# start DEV server
./bin/dev

rails g stimulus toggle

rails stimulus:manifest:update


```