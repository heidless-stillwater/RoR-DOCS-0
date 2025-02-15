
# [Active Record Associations](https://guides.rubyonrails.org/association_basics.html)

- ## [2.4 The has_many :through Association](https://guides.rubyonrails.org/association_basics.html#the-has-many-through-association)

```
rails generate migration create_article_categories

article = Article.last
category = Category.last

category.articles << article

category.articles << Article.first

article.categories << Category.first
article.categories << Category.last

article.categories.count

```

# alpha-blog: categories checkox example
- ## [alpha-blog-11-2017-5.1.4 : form checkbox](https://github.com/udemyrailscourse/alpha-blog-11-2017-5.1.4/blob/master/app/views/articles/_form.html.erb)

# [Rails Form helpers](https://guides.rubyonrails.org/v2.3/form_helpers.html)

- ## [3 Making Select Boxes with Ease](https://guides.rubyonrails.org/v2.3/form_helpers.html#making-select-boxes-with-ease)
