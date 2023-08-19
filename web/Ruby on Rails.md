# Ruby on Rails

## General
- A *full-stack web application development framework* written in the **Ruby** programming language
- Follows the **M**odel-**V**iew-**C**ontroller (**MVC**) application design pattern
- The **Rails** philosophy includes two major guiding principles:
  - **Don't Repeat Yourself:** *DRY* is a principle of software development which states that "Every piece of knowledge
    must have a single, unambiguous, authoritative representation within a system". By not writing the same information
    over and over again, our code is more maintainable, more extensible, and less buggy.
  - **Convention Over Configuration:** **Rails** has opinions about the best way to do many things in a web application,
    and defaults to this set of conventions, rather than require that we specify minutiae through endless configuration
    files.
- Installing **Rails** is as easy as running `gem install rails` on the command line
- The [**devise**](https://github.com/heartcombo/devise) gem is a popular choice for adding *authentication* to a **Rails** application

## Project Structure
- `app` - it organizes the application components into subdirectories
- `app/controllers` - the subdirectory where **Rails** looks to find the controller classes
- `app/helpers` - holds any helper classes used to assist the model, view, and controller classes
- `app/models` - holds the classes that model and wrap the data stored in our application's database
- `app/view` - holds the display templates to fill in with data from our application, convert to **HTML**, and return to
  the user's browser
- `app/view/layouts` - holds the template files for layouts to be used with views (e.g. *header*/*footer*)
- `components` - holds *components*, tiny self-contained applications that bundle model, view, and controller
- `config` - holds the configuration for the database (`database.yml`), the **Rails** environment structure (`environment.rb`)
  and routing of incoming web requests (`routes.rb`)
- `db` - holds scripts we create to manage relational database tables in the application
- `doc` - holds all the **RubyDoc**-generated **Rails** and application documentation
- `lib` - holds application-specific libraries
- `log` - holds error logs for the server (`server.log`) and each **Rails** environment (`development.log`, `test.log`,
  and `production.log`)
- `public` - holds web files that don't change, such as **JavaScript** files (`public/javascripts`), graphics
  (`public/images`), stylesheets (`public/stylesheets`) and **HTML** files (`public`)
- `script` - holds scripts to launch and manage the various tools we'll use with **Rails**
- `test` - holds mocks (`mocks`), unit tests (`unit`), fixtures (`fixtures`) and functional tests (`functional`)
- `tmp` - holds temporary files for intermediate processing
- `vendor` - holds libraries provided by third-party vendors (such as security libraries or database utilities beyond
  the basic **Rails** distribution)
- `README` - this file contains details about the application and a description of the directory structure explained
  here
- `Rakefile` - this file is used by the `rake` utility that is shipped with the **Ruby** installation and helps with
  building, packaging and testing the code (similar to a **Unix Makefile**)
- `Gemfile` - the file where all our application's gem dependencies are declared in a structured way

## The Rails Command Line
The `rails` tool offers several useful commands for managing a **Rails** application:

- `rails new foo`  
  Creates a new **Rails** application in subdirectory `foo`.

- `rails s[erver]`  
  Starts up a Web server on port *3000* running the current application; log messages from the server will appear on
  standard output.

- `rails console`  
  Starts up a **Rails** application in interactive mode; doesn't actually start a web server.

- `rails g[enerate] controller foo a b`  
  Creates a new controller class `FooController` with a skeleton class definition in
  `app/controllers/foo_controller.rb`. It also creates skeleton action methods `a` and `b` in the controller, plus
  skeleton views in the files `app/views/foo/a.html.erb` and `app/views/foo/a.html.erb`. If `a` and `b` are omitted then
  the controller is created with no actions or views.

- `rails g[enerate] model foo`  
  Creates a new model class `Foo` with a skeleton class definition in `app/models/foo.rb` and a skeletal migration in
  `db/migrate/*_create_foos.rb`.

- `rails g[enerate] migration foo`  
  Creates a new database migration in the file `db/migrate/*_foo.rb`.

- `rails destroy model foo`  
  Deletes all of the files created by a `rails generate model foo` command. This command has similar forms to match all
  of the other `rails generate` commands.

- `rails routes`  
  List all defined routes.

Some other useful commands available in a **Rails** project include:

- `rake db:migrate`  
  Runs all migrations to bring the database up to date.

- `rake db:reset`  
  Drops the database and creates a new one (does not run any migrations on the new database).

- `rake db:migrate:reset`  
  Drops the database, creates a new one, and runs all migrations to bring the database up to date.

- `rake db:migrate VERSION=20090909201633_create_photos.rb`  
  Runs migrations (either forward or backward) to restore the database to match the state just after the given
  migration.

- `bundle update`  
  If we modify the `Gemfile` in a project in order to include new or different ***Ruby Gems***, this command will update
  all of our **Gems** to match the `Gemfile`.

- `gem update`  
  Updates all ***Ruby Gems*** to their latest versions.

- `gem update rails --include-dependencies`  
  Updates **Rails** to the latest version, including all **Gems** that **Rails** depends upon.

- `gem update --system`  
  Updates **Ruby** to the latest version.

## Model
**Active Record** is the **M** in **MVC** - the model - which is the layer of the system responsible for representing
business data and logic. **Active Record** facilitates the creation and use of business objects whose data requires
persistent storage to a database. It is an implementation of the **Active Record** pattern which itself is a description
of an **O**bject **R**elational **M**apping (**O/RM**) system.

By default, **Active Record** uses some naming conventions to find out how the mapping between models and database
tables should be created. **Rails** will pluralize our class names to find the respective database table. So, for a
class `Book`, we should have a database table called `books`. The **Rails** pluralization mechanisms are very powerful,
being capable of pluralizing (and singularizing) both regular and irregular words. When using class names composed of
two or more words, the model class name should follow the **Ruby** conventions, using the *CamelCase* form, while the
table name must use the *snake_case* form.

**Examples:**

| Model / Class | Table / Schema |
|:-------------:|:--------------:|
| `Article`     | `articles`     |
| `LineItem`    | `line_items`   |
| `Deer`        | `deers`        |
| `Mouse`       | `mice`         |
| `Person`      | `people`       |

### Associations
In **Rails**, an *association* is a connection between two **Active Record** models:

```ruby
class Author < ApplicationRecord
  has_many :books, dependent: :destroy
end

class Book < ApplicationRecord
  belongs_to :author
end
```

### Migrations
Migrations are a convenient way to alter our database schema over time in a consistent way. They use a **Ruby DSL** so
that we don't have to write **SQL** by hand, allowing our schema and changes to be database independent.

We can think of each migration as being a new 'version' of the database. A schema starts off with nothing in it, and
each migration modifies it to add or remove tables, columns, or entries. **Active Record** knows how to update our
schema along this timeline, bringing it from whatever point it is in the history to the latest version. **Active
Record** will also update our `db/schema.rb` file to match the up-to-date structure of our databse.

We can create a *migration* using the command `rails g[enerate] migration <name> <field>:<type>[:index]...`.

For example:

```sh
rails generate migration AddPartNumberToProducts part_number:string
```

Migrations use naming conventions to generate the appropriate method calls (e.g. `CreateXXX`, `AddColumnToTable`,
`RemoveColumnFromTable`, etc.).

## View
In **Rails**, web requests are handled by **Action Controller** and **Action View**. Typically, **Action Controller** is
concerned with communicating with the database and performing **CRUD** actions where necessary. **Action View** is then
responsible for compiling the response.

**Action View** templates are written using embedded **Ruby** in tags mingled with **HTML**. They are defined in
`.html.erb` files.

Embedded **Ruby** goes between `<% %>` tags (or `<%= %>` tags which also output the result):

```erb
<h1>Articles</h1>

<ul>
  <% @articles.each do |article| %>
    <li>
      <a href="/articles/<%= article.id %>">
        <%= article.title %>
      </a>
    </li>
  <% end %>
</ul>
```

### Partials
Partials are layout files with names that start with an underscore (e.g. `_header.html.erb`) that are meant to be shared
by multiple pages.

They are defined in `app/views/shared` and rendered using the `render` command.

```erb
<%= render "home/header" %>
```

### Helpers
A *helper* is a method that is (mostly) used in our **Rails** views to share reusable code.

**Rails** comes with a set of useful built-in helper methods, such as:

```erb
<%= link_to "Profile", profile_path(@profile), id: "news", class: "article" %>
```

which results in:

```html
<a href="/profiles/1" id="news" class="article">Profile</a>
```

We can define custom helper methods in `app/helpers`.

## Controller
**Action Controller** is the **C** in **MVC**. After the router has determined which controller to use for a request,
the controller is responsible for making sense of the request and producing the appropriate output.

The controller can be thought of as a middleman between models and views. It makes the model data available to the view,
so it can display that data to the user, and it saves or updates user data to the model.

Every controller derives from the **Rails** `ApplicationController` class:

```ruby
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end

  def show
    @article = Article.find(params[:id])
  end
end
```

### Filters
Filters are methods that are run "before", "after" or "around" a controller action.

Filters are inherited, so if we set a filter on `ApplicationController`, it will be run on every controller in our
application.

"before" filters are registered via `before_action`. They may halt the request cycle. A common "before" filter is one
which requires that a user is logged in for an action to be run. We can define the filter method this way:

```ruby
class ApplicationController < ActionController::Base
  before_action :require_login

  private

  def require_login
    unless logged_in?
      flash[:error] = "You must be logged in to access this section"
      redirect_to new_login_url # halts request cycle
    end
  end
end
```

We can pass an array of controller actions to the `only` option to register a filter only for those actions:

```ruby
before_action :require_login, only: [:edit]
```

## Routing
The **Rails** router recognizes **URLs** and dispatches them to a controller's action, or to a **Rack** application. It
can also generate paths and **URLs**, avoiding the need to hardcode strings in our views.

We define routes in `config/routes.rb`.

Each route definition is of the form `<verb> <path>, to: <controller>#<action>`, where:

- `<verb>` is an **HTTP** method verb (`get`, `post`, `patch`, `put`, `delete`)
- `<path>` is the request path
- `<controller>` is the name of a controller (in lowercase and without the *Controller* suffix)
- `<action>` is an action (a method) inside that controller

For example, the route `get "/articles", to: "articles#index"` declares that `GET /articles` requests are mapped to the
`index` action of `ArticlesController`.

To define the `root` route we can use `root <controller>#<action>` instead.

### Route Parameters
A *route parameter* captures a segment of the request's path, and puts that value into the `params` **Hash**, which is
accessible by the controller action.

For example, let's say we have defined the route `get "/articles/:id", to: "articles#show"` in `config/routes.rb`.

Then, when handling a request like `GET http://localhost:3000/articles/1`, `1` would be captured as the value for `:id`,
which would then be accessible as `params[:id]` in the `show` action of `ArticlesController`.

### Resource Routing
*Resource routing* (the **Rails** default) allows us to quickly declare all of the common routes for a given resourceful
controller.

A *resourceful route* provides a mapping between **HTTP** verbs and **URLs**, and controller actions.

By convention, each action also maps to particular **CRUD** operations in a database.

A single entry in the routing file, such as:

```ruby
resources :photos
```

creates seven different routes in our application, all mapping to the `Photos` controller:

```ruby
GET       /photos
GET       /photos/new
POST      /photos
GET       /photos/:id
GET       /photos/:id/edit
PATCH/PUT /photos/:id
DELETE    /photos/:id
```

When creating a *resourceful route*, **Rails** exposes a number of helpers to the controllers of an application.

For example, in the case of `resources :photos`:

- `photos_path` returns `/photos`
- `new_photo_path` returns `/photos/new`
- `edit_photo_path(:id)` returns `/photos/:id/edit`
- `photo_path(:id)` returns `/photos/:id`

Each of these helpers has a corresponding `_url` helper (such as `photos_url`) which returns the same path prefixed with
the current host, port, and path prefix.

## Scaffold
A *scaffold* is a full set of model, database migration for that model, controller to manipulate it, views to view and
manipulate the data, and a test suite for each of the above.

We can create a scaffold with `rails g[enerate] scaffold <model-name> <field-name>:<data-type>...`.

For example:

```sh
rails g scaffold Task name:string done:boolean due:datetime
```

Running `rails db:migrate` right after, will create the database table for the model.
