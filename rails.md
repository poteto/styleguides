# Rails Style Conventions

## Table of Contents
1.  [Routing](#routing)
1.  [Controllers](#controllers)
1.  [Models](#models)
    1. [Naming Conventions](#naming-conventions)
    1. [ActiveRecord](#activerecord)
    1. [ActiveRecord Queries](#activerecord-queries)
1.  [Migrations](#migrations)
1.  [Views](#views)
1.  [Testing](#testing)
1.  [Gems](#gems)

## Routing

*  When you need to add more actions to a RESTful resource, use `member` and `collection` routes.

```ruby
# bad
get 'subscriptions/:id/unsubscribe'
resources :subscriptions

# good
resources :subscriptions do
  get 'unsubscribe', on: :member
end
```

```ruby
# bad
get 'photos/search'
resources :photos

# good
resources :photos do
  get 'search', on: :collection
end
```

* If you need to define multiple `member/collection` routes use the
  alternative block syntax.

```ruby
resources :subscriptions do
  member do
    get 'unsubscribe'
    # more routes
  end
end

resources :photos do
  collection do
    get 'search'
    # more routes
  end
end
```

* Use nested routes to clearly represent the relationship between ActiveRecord models.

```ruby
class Post < ActiveRecord::Base
  has_many :comments
end

class Comments < ActiveRecord::Base
  belongs_to :post
end

# routes.rb
resources :posts do
  resources :comments
end
```

* Use namespaced routes to group related actions.

```ruby
namespace :admin do
  resources :products
end
```

## Controllers

*  Keep the controllers light. They shouldn't contain any business logic. All the business
   logic should reside in the model.

## Models

### Naming Conventions

* By default, Active Record uses some naming conventions to find out how
the mapping between models and database tables should be created.

* Rails will pluralize your class names to find the respective database
table.
  * Active Record Database Table: snake case (e.g., `book_clubs`).
  * Model Class: camel case (e.g., `BookClub`).
  * Constants: all uppercase and snake case if multiple works (e.g.,
    BOOK_CLUBS)

* Name the models with meaningful (but short) names without abbreviations.

### ActiveRecord

* Avoid altering ActiveRecord defaults (table names, primary key, etc)
  unless you have a very good reason.

* Group macro-style methods (`has_many`, `validates`, etc) in the beginning
   of the class definition.

```ruby
class User < ActiveRecord::Base
  # keep the default scope first (if any)
  default_scope { where(active: true) }

  # constants come up next
  COLORS = %w(red green blue)

  # followed by association macros
  belongs_to :country
  has_many :authentications, dependent: :destroy

  # and validation macros
  validates :email, presence: true
  validates :username, presence: true
  validates :username, uniqueness: { case_sensitive: false }

  # next we have callbacks
  before_save :cook
  before_save :update_username_lower

  ...
end
```

### ActiveRecord Queries

* Favor the use of `find` over `where` when you need to retrieve a single
record by id.

```ruby
# bad
User.where(id: id).take

# good
User.find(id)
```

* Favor the use of `find_by` over `where` when you need to retrieve a single
record by some attributes.

```ruby
# bad
User.where(first_name: 'Bruce', last_name: 'Lee').first

# good
User.find_by(first_name: 'Bruce', last_name: 'Lee')
```

* Use `find_each` when you need to process a lot of records.

```ruby
# bad - loads all the records at once
User.all.each do |user|
  NewsMailer.weekly(user).deliver_now
end

# good - records are retrieved in batches
User.find_each do |user|
  NewsMailer.weekly(user).deliver_now
end
```

* Favor the use of where.not over SQL.

```ruby
# bad
User.where("id != ?", id)

# good
User.where.not(id: id)
```

## Migrations

* Set default values in the migrations themselves instead of in the
application layer.

```ruby
# bad
def price
  self[:price] or 0
end

# good
class CreateProducts < ActiveRecord::Migration
  def change
    create_table :products do |t|
      t.string :name
      t.integer :price, default: 0
    end
  end
end
```

* When adding tables or columns, use the `change` method instead of `up` and `down` methods.

```ruby
# the old way
class AddNameToPeople < ActiveRecord::Migration
  def up
    add_column :people, :name, :string
  end

  def down
    remove_column :people, :name
  end
end

# the new prefered way
class AddNameToPeople < ActiveRecord::Migration
  def change
    add_column :people, :name, :string
  end
end
```

## Views

* If in need of a view layer, use ERB templating over HAML.

* Use double quotes when dealing with HTML or string interpolation.

* Never make complex formatting in the views, export the formatting to a
method in the view helper or the model.

* Mitigate code duplication by using partial templates and layouts.

## Testing

TODO: fill this in.

## Gems

### Grouping your dependencies

Place gems in their appropriate group environment. Bundler provides a `groups` feature
that allows you to seperate your gem dependencies into environments that
make the most sense.

```ruby
# bad
gem 'byebug'

# good in-line syntax
gem 'byebug', group: :development

# good block syntax
group :development do
  gem 'byebug'
end
```

## Useful gems

### General

* [`active_model_serializers`](https://github.com/rails-api/active_model_serializers): Brings
  convention over configuration to your JSON generation.

* [`carrierwave`](https://github.com/carrierwaveuploader/carrierwave): Proves a simple
  and extremely flexible way to upload files.

* [`context_validations`](https://github.com/dockyard/ruby-context_validations): Context
  based validations for model instances.

* [`destroyed_at`](https://github.com/dockyard/ruby-destroyed_at): Allows you to
  "destroy" an object without deleting the record or assocaited records.

* [`easy_auth`](https://github.com/dockyard/ruby-easy_auth): Simple drop in authentication
  for Rails 3.2+

  * To be used in conjunction with other auth gems found
    [here](https://github.com/dockyard/ruby-easy_auth/wiki/Plugins).

* [`friendly_id`](https://github.com/norman/friendly_id): Addon to Ruby's Active Record
  that allows you to replace ids in your URLs with strings.

* [`geocoder`](https://github.com/alexreisner/geocoder): Complete geocoding solution for Ruby.

* [`pages`](https://github.com/dockyard/pages): Simple dynamic yet static pages.

* [`party_foul`](https://github.com/dockyard/party_foul): Rails exceptions automatically
  opened as issues on GitHub.

* [`pg`](https://github.com/ged/ruby-pg): Ruby interface to the PostgreSQL RDBMS.

  By default, Rails uses the SQLite3 Database. To begin a Rails
  project with Postgres, run the following command:

  * `rails new myproject --database=postgresql`

* [`postgres_ext`](https://github.com/dockyard/postgres_ext): Adds missing native PostgreSQL data
  types to ActiveRecord and convenient querying extensions for ActiveRecord and Arel for Rails 4.x

* [`postgres_ext-postgis`](https://github.com/dockyard/postgres_ext-postgis): Extends
  ActiveRecord's data type handling and query methods in both Arel and ActiveRecord.

* [`postgres_ext-serializers`](https://github.com/dockyard/postgres_ext-serializers): Will take
  over anytime you try to serialize an ActiveRecord::Relation.

* [`will_paginate`](https://github.com/mislav/will_paginate): Pagination library.

### Development

* [`bullet`](https://github.com/flyerhzm/bullet): Helps increase your application's
  performance by reducing the number of queries it makes.

* [`byebug`](https://github.com/deivid-rodriguez/byebug): Debugger.

* [`quiet_assets`](https://github.com/evrone/quiet_assets): Turns off the Rails asset pipeline log.

* [`thin`](https://github.com/macournoyer/thin/): Tiny, fast, and funny HTTP server.

### Testing

* [`capybara`](https://github.com/jnicklas/capybara): Simulates how a real user would
  interact with your app.

* [`capybara-email`](https://github.com/dockyard/capybara-email): Easily test ActionMailer
  and Mail messages in your Capybara integration tests.

* [`capybara-extensions`](https://github.com/dockyard/capybara-extensions): Extends
  Capybara's finders and matchers with additional methods for interacting with
  tables, lists, list items, and many HTML5 elements

* [`database_cleaner`](https://github.com/DatabaseCleaner/database_cleaner): Set of strategies
  for cleaning your database; ensures clean state during tests.

* [`m`](https://github.com/qrush/m): Test runner that can run tests by line number.

* [`minitest-spec-rails`](https://github.com/metaskills/minitest-spec-rails): Makes it easy
  to use the MiniTest::Spec DSL.

* [`valid_attribute`](https://github.com/bcardarella/valid_attribute): Minimalist matcher for validation.

### Production

* [`fog`](https://github.com/fog/fog): Cloud services library.

* [`unicorn`](https://github.com/defunkt/unicorn): Rack HTTP server for fast clients and Unix.
