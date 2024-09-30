
# git
```
# git branch -d <BRANCH-NAME>

# create branch
git checkout -b <new BRANCH-NAME>>

# commit checnges
git add -A
git commit -m <message>

# merge changes
git checkout main
git merge <BRANCH-NAME>

# push to remote
git push origin main

```

# rvm 
```
rvm list

rvm install 2.6.0

```

# rails
```
rails new alpha-blog

rails s

rails generate controller pages

rails generate scaffold Article title:string description:text 
rails generate migration add_user_id_to_articles

rails db:migrate 
# rails db:reset

rails routes --expanded

rails generate migration create_articles

rails db:rollback

rails db:drop

```

# authentication
```

BCrypt::Password.create("password")
password = _
password
password.salt

user = User.last
user.password = "password123"
user.authenticate("wronPassword")
user.authenticate("password123")

```


# examples
```
rails generate scaffold Article title:string description:text

rails generate scaffold User username:string description:text 

rails generate migration remove_user_description

rails generate migration add_user_id_to_articles

User.create(username: "heidless")

User.create(username: "havana")

user_1 = User.first
Article.create(title: "some title 1", description: "some title DESC", user: user_1)

user_1.articles.build(title: "some title 3", description: "some title DESC")
article = _   # "_" execute entire [revious command
article.save
Article.last

user_2 = User.last
article = Article.first
user_2.articles << article     # '<<'' the 'shovel' operator

user_1.valid?
user_1.errors
user_1.errors.full_messages

Article.update_all(user_id: User.first.id)

```

# rails - console
```
rails console

reload!

article.errors
article.errors.full_messages

Article.find(14)

User.create(username: "heidless")


```
