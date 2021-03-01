# Scheeds

Update seeds.rb in reference to schema.rb.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'scheeds'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install scheeds

## Usage

`db/schema.rb`:

```ruby
ActiveRecord::Schema.define(version: 2021_03_01_000001) do
  ...

  create_table "user", force: :cascade do |t|
    t.string "email", null: false
    t.string "name", null: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "article", force: :cascade do |t|
    t.string "title", null: false
    t.bigint "user_id", null: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["user_id"], name: "index_articles_on_user_id"
  end

  create_table "comments", force: :cascade do |t|
    t.string "body", null: false
    t.bigint "user_id", null: false
    t.bigint "article_id", null: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["user_id"], name: "index_comments_on_user_id"
    t.index ["article_id"], name: "index_comments_on_article_id"
  end

  add_foreign_key "articles", "users"
  add_foreign_key "comments", "users"
  add_foreign_key "comments", "articles"
end
```

Update `db/seeds.rb`

```sh
$ rake db:scheeds
```

Result:

```ruby
Comment.delete_all
Article.delete_all
User.delete_all

User.create!([
  { email: Faker::Internet.email, name: Faker::Name.name },
  { email: Faker::Internet.email, name: Faker::Name.name }
])

Article.create!([
  { title: Faker::Book.title, user_id: 1 },
  { title: Faker::Book.title, user_id: 2 }
])

Comment.create!([
  { body: Faker::Lorem.sentence, user_id: 1, article_id: 1 },
  { body: Faker::Lorem.sentence, user_id: 2, article_id: 2 }
])
```

### Settings

```ruby
# config/initializers/scheeds.rb

Scheeds.configure do |config|
  config.faker_locale = :ja
end
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/[USERNAME]/scheeds.


## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
