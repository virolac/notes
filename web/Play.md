# Play

## Introduction
- A high-productivity **Java** and **Scala** full-stack web application framework
- Model-View-Controller (**MVC**) architecture
- Uses **Akka** and **Akka Streams** under the covers to provide predictable and minimal resource consumption (**CPU**,
  memory, threads)
- **Play** applications scale naturally--both horizontally and vertically--thanks to its reactive model

## Requirements
A **Play** application only needs to include the **Play JAR** files to run properly. These  files are published to the
[**Maven Repository**](https://mvnrepository.com/), therefore we can use any **Java** or **Scala** build tool to build a
**Play** project. However, **Play** provides an enhanced development experience (support for routes, templates
compilation and auto-reloading) when using **sbt**.

**Play** requires:

1. **Java** versions **SE 8** through **SE 11**, inclusive
2. **sbt** (latest version recommended)

## Creating a new application
**Play** expects a specific project structure. We can use a [**giter8**](http://www.foundweekends.org/giter8/) to create
a new **Play** project. This gives us the advantage of setting up our project folders, build structure, and development
environment--all with one command.

To create a new project, we type the following to a command window:

```sh
sbt new playframework/play-scala-seed.g8
```

After the template creates the project:

1. We enter `sbt run` to download dependencies and start the system
2. In a browser, we enter `http://localhost:9000/` to view the welcome page

## Anatomy of a Play application
### The Play application layout
The layout of a **Play** application is standardized to keep things as simple as possible. After the first successful
compilation, the project structure looks like this:

```
app                      → Application sources
 └ assets                → Compiled asset sources
    └ stylesheets        → Typically LESS CSS sources
    └ javascripts        → Typically CoffeeScript sources
 └ controllers           → Application controllers
 └ models                → Application business layer
 └ views                 → Templates
build.sbt                → Application build script
conf                     → Configurations files and other non-compiled resources (on classpath)
 └ application.conf      → Main configuration file
 └ routes                → Routes definition
dist                     → Arbitrary files to be included in our projects distribution
public                   → Public assets
 └ stylesheets           → CSS files
 └ javascripts           → Javascript files
 └ images                → Image files
project                  → sbt configuration files
 └ build.properties      → Marker for sbt project
 └ plugins.sbt           → sbt plugins including the declaration for Play itself
lib                      → Unmanaged library dependencies
logs                     → Logs folder
 └ application.log       → Default log file
target                   → Generated stuff
 └ resolution-cache      → Info about dependencies
 └ scala-2.13
    └ api                → Generated API docs
    └ classes            → Compiled class files
    └ routes             → Sources generated from routes
    └ twirl              → Sources generated from templates
 └ universal             → Application packaging
 └ web                   → Compiled web assets
test                     → Source folder for unit or functional tests
```

- ### The `app/` directory
  The `app` directory contains all executable artifacts: **Java** and **Scala** source code, templates and compiled
  assets' sources.

  There are three packages in the `app` directory, one for each component of the **MVC** architectural pattern:

  - `app/controllers`
  - `app/models`
  - `app/views`

  We can add our own packages, for example, an `app/services` package.

  There is also an optional directory called `app/assets` for compiled assets such as **LESS** sources and
  **CoffeeScript** sources.

  <small>**Note:** in **Play**, the `controllers`, `models`, and `views` package names are simply conventions that can be
  changed if needed (such as prefixing everything with `com.mycompany`).</small>

- ### The `public/` directory
  Resources stored in the `public` directory are static assets that are served directly by the **Web** server.

  This directory is split into three sub-directories for images, **CSS** stylesheets and **JavaScript** files. We should
  organize our static assets like this to keep all **Play** applications consistent.

  <small>In a newly-created application, the `/public` directory is mapped to the `/assets` **URL** path, but we can
  easily changed that, or even use several directories for our static assets.</small>

- ### The `conf/` directory
  The `conf` directory contains the application's configuration files. There are two main configuration files:

  - `application.conf` is the main configuration file for the application
  - `routes` is the routers' definition file

  <br>

  If we need to add configuration options that are specific to our application, it's a good idea to add more options to
  the `application.conf` file.

  If a library needs a specific configuration file, it is good to provide it under the `conf` directory.

- ### The `lib/` directory
  The `lib` directory is optional and contains unmanaged library dependencies, i.e. all **JAR** files we want to
  manually manage outside the build system. We just drop any **JAR** files here and they are added to our application
  classpath.

- ### The `build.sbt` file
  Our project's main build declarations are generally found in `build.sbt` at the root of the project.

- ### The `project/` directory
  The `project` directory contains the **sbt** build definitions:

  - `plugins.sbt` defines **sbt** plugins used by this project
  - `build.properties` contains the **sbt** version to use to build our app

- ### The `target/` directory
  The `target` directory contains everything generated by the build system. It can be useful to know what is generated
  here:

  - `classes/` contains all compiled classes (from both **Java** and **Scala** sources).
  - `classes_managed/` contains only the classes that are managed by the framework (such as the classes generated by the
    router or the template system). It can be useful to add this class folder as an external class folder in our **IDE**
    project.
  - `resources_managed/` contains generated resources, typically compiled assets such as **LESS CSS** and
    **CoffeeScript** compilation results.
  - `src_managed/` contains generated sources, such as the **Scala** sources generated by the template system.
  - `web/` contains assets processed by [**sbt-web**](https://github.com/sbt/sbt-web#sbt-web) such as those from the
    `app/assets` and `public` folders.

- ### Typical `.gitignore` file
  Generated folders should be ignored by our version control system. Here is the typical `.gitignore` file for a
  **Play** application:

  ```.gitignore
  logs
  project/project
  project/target
  target
  tmp
  dist
  .cache
  RUNNING_PID
  ```

## Using the sbt console
We can manage the complete development cycle of a **Play** application with **sbt**. **sbt** has an interactive mode
(`shell`), or we can enter commands one at a time. The interactive mode can be faster over time because **sbt** only
needs to start once. When we enter commands one at a time, **sbt** restarts each time we run it.

### Single commands
We can run single **sbt** commands directly. For example, to build and run **Play**, we change to the directory of our
project and run:

```sh
sbt run
```

We will see something like:

```
[info] Loading project definition from /Users/play-developer/my-first-app/project
[info] Set current project to my-first-app (in build file:/Users/play-developer/my-first-app/)

--- (Running the application from sbt, auto-reloading is enabled) ---

[info] play - Listening for HTTP on /0:0:0:0:0:0:0:0:9000

(Server started, use Enter to stop and go back to the console...)
The application starts directly. When you quit the server using Ctrl+D or Enter, the command prompt returns.
```

### Interactive mode
To launch **sbt** in the interactive mode, we change into the top-level of our project and enter **sbt** with no
arguments:

```sh
$ cd my-first-app
my-first-app $  sbt
```

We will see something like:

```
[info] Loading global plugins from /Users/play-developer/.sbt/0.13/plugins
[info] Loading project definition from /Users/play-developer/my-first-app/project
[info] Updating {file:/Users/play-developer/my-first-app/project/}my-first-app-build...
[info] Resolving org.fusesource.jansi#jansi;1.4 ...
[info] Done updating.
[info] Set current project to my-first-app (in build file:/Users/play-developer/my-first-app/)
[my-first-app] $
```

<small>**Tip:** We can also launch some commands before getting into **sbt** shell by running shell at the end of task
list. For example:

```sh
$ sbt clean compile shell
```
</small>

### Development mode
In this mode, **sbt** launches **Play** with the auto-reload feature enabled. When we make a request, **Play** will
automatically recompile and restart our server if any files have changed. If needed, the application will restart
automatically.

With **sbt** in the interactive mode, to run the current application in development mode, we use the `run` command:

```sh
[my-first-app] $ run
```

We will see something like:

```
$ sbt
[info] Loading global plugins from /Users/play-developer/.sbt/1.0/plugins
[info] Loading project definition from /Users/play-developer/tmp/my-first-app/project
[info] Done updating.
[info] Loading settings for project root from build.sbt ...
[info] Set current project to my-first-app (in build file:/Users/play-developer/tmp/my-first-app/)
[info] sbt server started at local:///Users/play-developer/.sbt/1.0/server/c9c53f40a402da68f71a/sock
[my-first-app] $ run
[info] Updating ...
[info] Done updating.
[warn] There may be incompatibilities among your library dependencies; run 'evicted' to see detailed eviction warnings.

--- (Running the application, auto-reloading is enabled) ---

[info] p.c.s.AkkaHttpServer - Listening for HTTP on /0:0:0:0:0:0:0:0:9000

(Server started, use Enter to stop and go back to the console...)
```

### Compiling only
We can also compile our application without running the **HTTP** server. The `compile` command displays any application
errors in the command window. For example, in the interactive mode, we can enter:

```sh
[my-first-app] $ compile
```

And we will see something like:

```
[my-first-app] $ compile
[info] Compiling 1 Scala source to /Users/play-developer/my-first-app/target/scala-2.13/classes...
[error] /Users/play-developer/my-first-app/app/controllers/HomeController.scala:21: not found: value Actionx
[error]   def index = Actionx { implicit request =>
[error]               ^
[error] one error found
[error] (compile:compileIncremental) Compilation failed
[error] Total time: 1 s, completed Feb 6, 2017 2:00:07 PM
[my-first-app] $
```

If there are no errors with our code, we will see:

```
[my-first-app] $ compile
[info] Updating {file:/Users/play-developer/my-first-app/}root...
[info] Resolving jline#jline;2.12.2 ...
[info] Done updating.
[info] Compiling 8 Scala sources and 1 Java source to /Users/play-developer/my-first-app/target/scala-2.13/classes...
[success] Total time: 3 s, completed Feb 6, 2017 2:01:31 PM
[my-first-app] $
```

### Testing options
We can run tests without running the server. For example, in interactive mode, we can use the `test` command:

```sh
[my-first-app] $ test
```

The `test` command will run all the tests in our project. We can also use `testOnly` to select specific tests:

```sh
[my-first-app] $ testOnly com.acme.SomeClassTest
```

### Launching the Scala console
We can type `console` to enter the **Scala** console, which allows us to test our code interactively:

```sh
[my-first-app] $ console
```

To start the application inside the **Scala** console (e.g. to access the database):

```scala
import play.api._
val env     = Environment(new java.io.File("."), this.getClass.getClassLoader, Mode.Dev)
val context = ApplicationLoader.Context.create(env)
val loader  = ApplicationLoader(context)
val app     = loader.load(context)
Play.start(app)
```

### Debugging
We can ask **Play** to start a **JPDA** debug port when starting the console. We can then connect using the **Java**
debugger. We can use the `sbt -jvm-debug <port>` command to do that:

```sh
$ sbt -jvm-debug 9999
```

When a **JPDA** port is available, the **JVM** will log this line during boot:

```
Listening for transport dt_socket at address: 9999
```

### Using sbt features
We can use sbt features such as **triggered execution**.

For example, using `~ compile`:

```sh
[my-first-app] $ ~ compile
```

The compilation will be triggered each time we change a source file.

If we are using `~ run`:

```sh
[my-first-app] $ ~ run
```

The triggered compilation will be enabled while a development server is running.

We can also do the same for `~ test`, to continuously test our project each time we modify a source file:

```sh
[my-first-app] $ ~ test
```

This could be especially useful if we want to run just a small set of our tests using the `testOnly` command. For
instance:

```sh
[my-first-app] $ ~ testOnly com.acme.SomeClassTest
```

Will trigger the execution of the `com.acme.SomeClassTest` test every time we modify a source file.

### Using the Play commands directly
We can also run commands directly without entering the **Play** console. For example, we can enter `sbt run`:

```sh
$ sbt run
[info] Loading project definition from /Users/play-developer/my-first-app/project
[info] Set current project to my-first-app (in build file:/Users/play-developer/my-first-app/)

--- (Running the application from sbt, auto-reloading is enabled) ---

[info] play - Listening for HTTP on /0:0:0:0:0:0:0:0:9000

(Server started, use Enter to stop and go back to the console...)
```

The application starts directly. When we quit the server using `Ctrl+D` or `Enter`, we will come back to our **OS**
prompt.

By default, the server runs on port `9000`. A custom port (e.g. `8080`) can be specified:

```sh
sbt 'run 8080'
```

Of course, the **triggered execution** is available here as well:

```sh
sbt ~run
```

### Getting help
We can use the `help` command to get basic help about the available commands. We can also use this with a specific
command to get information about that command:

```sh
[my-first-app] $ help run
```

## Actions, Controllers and Results
### What is an Action?
Most of the requests received by a **Play** application are handled by an `Action`.

A `play.api.mvc.Action` is basically a `(play.api.mvc.Request => play.api.mvc.Result)` function that handles a request
and generates a result to be sent to the client.

```scala
def echo = Action { request =>
  Ok("Got request [" + request + "]")
}
```

An action returns a `play.api.mvc.Result` value, representing the **HTTP** response to send to the web client. In this
example `Ok` constructs a **200 OK** response containing a `text/plain` response body.

### Building an Action
Within any controller extending `BaseController`, the `Action` value is the default action builder. This action builder
contains several helpers for creating `Action`s.

The first simplest one just takes as argument an expression block returning a `Result`:

```scala
Action {
  Ok("Hello world")
}
```

This is the simplest way to create an `Action`, but we don't get a reference to the incoming request. It is often useful
to access the **HTTP** request calling this `Action`.

So there is another `Action` builder that takes as an argument a function `Request => Result`:

```scala
Action { request =>
  Ok("Got request [" + request + "]")
}
```

It is often useful to mark the `request` parameter as `implicit` so it can be implicitly used by other **APIs** that
need it:

```scala
Action { implicit request =>
  Ok("Got request [" + request + "]")
}
```

If we have broken up our code into methods, then we can pass through the implicit request from the action:

```scala
def action = Action { implicit request =>
  anotherMethod("Some para value")
  Ok("Got request [" + request + "]")
}

def anotherMethod(p: String)(implicit request: Request[_]) = {
  // do something that needs access to the request
}
```

The last way of creating an `Action` value is to specify an additional `BodyParser` argument:

```scala
Action(parse.json) { implicit request =>
  Ok("Got request [" + request + "]")
}
```

### Controllers are action generators
A controller in **Play** is nothing more than an object that generates `Action` values. Controllers are typically
defined as classes to take advantage of **Dependency Injection**.

<small>**Note:** Defining controllers as objects will not be supported in future versions of **Play**. Using classes is
the recommended approach.</small>

The simplest use case for defining an action generator is a method with no parameters that returns an `Action` value:

```scala
package controllers

  import javax.inject.Inject

  import play.api.mvc._

  class Application @Inject() (cc: ControllerComponents) extends AbstractController(cc) {
    def index = Action {
      Ok("It works!")
    }
  }
```

Of course, the action generator method can have parameters, and these parameters can be captured by the `Action`
closure:

```scala
def hello(name: String) = Action {
  Ok("Hello " + name)
}
```

### Simple results
For now we are just interested in simple results: an **HTTP** result with a status code, a set of **HTTP** headers and a
body to be sent to the web client.

These results are defined by `play.api.mvc.Result`:

```scala
import play.api.http.HttpEntity

def index = Action {
  Result(
    header = ResponseHeader(200, Map.empty),
    body = HttpEntity.Strict(ByteString("Hello world!"), Some("text/plain"))
  )
}
```

Of course there are several helpers available to create common results such as the `Ok` result in the sample above:

```scala
def index = Action {
  Ok("Hello world!")
}
```

This produces exactly the same result as before.

Here are several examples to create various results:

```scala
val ok           = Ok("Hello world!")
val notFound     = NotFound
val pageNotFound = NotFound(<h1>Page not found</h1>)
val badRequest   = BadRequest(views.html.form(formWithErrors))
val oops         = InternalServerError("Oops")
val anyStatus    = Status(488)("Strange response type")
```

All of these helpers can be found in the `play.api.mvc.Results` trait and companion object.

### Redirects are simple results too
Redirecting the browser to a new **URL** is just another kind of simple result. However, these result types don't take a
response body.

There are several helpers available to create redirect results:

```scala
def index = Action {
  Redirect("/user/home")
}
```

The default is to use a `303 SEE_OTHER` response type, but we can also set a more specific status code if we need one:

```scala
def index = Action {
  Redirect("/user/home", MOVED_PERMANENTLY)
}
```

### `TODO` dummy page
We can use an empty `Action` implementation defined as `TODO`: the result is a standard *"Not implemented yet"* result
page:

```scala
def index(name: String) = TODO
```

## Writing Play controllers in Scala
**Play** controllers are the most important part of any **Play** application. A **Play Scala** application shares the
same concepts as a classical **Play** application but uses a more functional way to describe actions.

### Scala controllers are Objects
A **Controller** is a **Scala** singleton object, hosted by the `controllers` package, and subclassing
`play.mvc.Controller`. In **Scala** we can declare as many controllers we want in the same file.

This is a classical controller definition:

```scala
package controllers {
  import play._
  import play.mvc._

  object Users extends Controller {
    def show(id:Long) = Template("user" -> User.findById(id))

    def edit(id:Long, email:String) = {
      User.changeEmail(id, email)
      Action(show(id))
    }

  }
}
```

Because **Scala** provides the native notion of **Singleton Objects** we don't need to deal with **Java** static methods
while keeping the ability to statically reference any action, like `show(id)`.

### Action methods return values
A **Play** controller usually uses imperative orders like `render(...)` or `forbidden()` to trigger the response
generation. On the contrary action methods written in **Scala** are seen as functions and must return a value. This
value will be used by the framework to generate the **HTTP** response result of the request.

An action method can of course return several kinds of values depending on the request (like for example a `Template` or
a `Forbidden` value).

Here is a list of the typical return types:

- #### Ok
  Returning the `Ok` value will generate an empty **200 OK** response.

  ```scala
  def index = Ok
  ```

- #### Template
  If an action method returns a `Template` value, the corresponding action template will be rendered, and a **200 OK**
  response filled with the generated content will be sent to the client.

  ```scala
  def index = Template
  ```

  By default, the template name will be resolved from the action method name. So if the `controllers.Application.index`
  method returns a `Template`, the `app/views/Application/index.html` template will be rendered.

  We can of course specify another template name:

  ```scala
  def index = Template("Commons/home.html")
  ```

  In this case the `app/views/Commons/home.html` template will be rendered.

  We can also pass values to be included during the template evaluation. Values are passed in the form of `(Symbol,
  Any)` tuples, where the first member will be used as a variable name in the template, and the second member as the
  variable value.

  ```scala
  def index = Template('now -> new Date(), 'numbers -> List(1,2,3))
  ```

- #### Html
  Returning an `Html` value will generate a **200 OK** response filled with the **HTML** content. The response content
  type will be automatically set to `text/html`.

  ```scala
  def index = Html("<h1>Hello world!</h1>")
  ```

- #### Xml
  Returning an `Xml` value will generate a **200 OK** response filled with the **XML** content. The response content
  type will be automatically set to `text/xml`.

  ```scala
  def index = Xml(<message>Hello world!</message>)
  ```

- #### Text
  Returning a `Text` value will generate a **200 OK** response filled with the text content. The response content type
  will be automatically set to `text/plain`.

  ```scala
  def index = Text("Hello world!")
  ```

- #### Json
  Returning a `Json` value will generate a **200 OK** response filled with the text content. The response content type
  will be automatically set to `application/json`.

  ```scala
  def index = Json("{ message: 'Hello world' }")
  ```

  We can also try to pass any **Scala** object and **Play** will try to serialize it to **JSON**:

  ```scala
  def index = Json(users)
  ```

  <small>Currently the **JSON** serialization mechanism comes from **Java** and can not work as expected with complex
  **Scala** structures.
  A workaround is to use a **Scala** dedicated **JSON** seriazilation library, for example [**Lift
  JSON**](https://github.com/jonifreeman/liftweb/tree/master/lift-json/), and use it as
  `Json(JsonAST.render(users))`.</small>

- #### Created
  Returning the `Created` value will generate an empty **201 Created** response.

  ```scala
  def index = Created
  ```

- #### Accepted
  Returning the `Accepted` value will generate an empty **202 Accepted** response.

  ```scala
  def index = Accepted
  ```

- #### NoContent
  Returning the `NoContent` value will generate an empty **204 No Content** response.

  ```scala
  def index = NoContent
  ```

- #### Action
  If an action method returns an `Action` value, **Play** will redirect the browser to the corresponding action, using
  the action method arguments to properly resolve the correct **URL**.

  ```scala
  def index = Action(show(3))
  ```

  Here `show(3)` is a **by-name** parameter, and the corresponding method will not be invoked. **Play** will resolve
  this call as a **URL** (typically something like `users/3`, and will issue an **HTTP** redirect to this **URL**. The
  action will then be invoked in a new request context.

  <small>In a **Java** controller we achieve the same result by calling the corresponding action method directly. Using
  the **Scala call-by-name** concept allows us to keep the compiler check and typesafe indirection without any language
  hack.</small>

- #### Redirect
  Returning the `Redirect` value will generate an empty **301 Moved Permanently** response.

  ```scala
  def index = Redirect("http://www.google.com")
  ```

  We can optionally specify a second argument to switch between **301** and **302** response status code.

  ```scala
  def index = Redirect("http://www.google.com", false)
  ```

- #### NotModified
  Returning the `NotModified` value will generate an empty **304 Not Modified** response.

  ```scala
  def index = NotModified
  ```

  We can also specify an **ETag** to the response:

  ```scala
  def index = NotModified("123456")
  ```

- #### BadRequest
  Returning the `BadRequest` value will generate an empty **400 Bad Request** response.

  ```scala
  def index = BadRequest
  ```

- #### Unauthorized
  Returning the `Unauthorized` value will generate an empty **401 Unauthorized** response.

  ```scala
  def index = Unauthorized
  ```

  We can optionally specify a realm name:

  ```scala
  def index = Unauthorized("Administration area")
  ```

- #### Forbidden
  Returning the `Forbidden` value will generate an empty **403 Forbidden** response.

  ```scala
  def index = Forbidden
  ```

  We can optionally specify an error message:

  ```scala
  def index = Forbidden("Insufficient permissions")
  ```

- #### NotFound
  Returning the `NotFound` value will generate an empty **404 Not Found** response.

  ```scala
  def index = NotFound
  ```

  We can optionally specify a resource name:

  ```scala
  def index = NotFound("Article not found")
  ```

  Or use a more classical (**HTTP** method, resource **Path**) combination:

  ```scala
  def index = NotFound("GET", "/toto")
  ```

- #### Error
  Returning the `Error` value will generate an empty **500 Internal Server Error** response.

  ```scala
  def index = Error
  ```

  We can optionally specify an error message:

  ```scala
  def index = Error("Oops...")
  ```

  Or specify a more specific error code:

  ```scala
  def index = Error(503, "Not ready yet...")
  ```

### Return type inference
We can also directly use the inferred return type to send the action result. For example using a `String`:

```scala
def index = "<h1>Hello world</h1>"
```

Or we can even use the built-in **XML** support to write **XHTML** in a literal way:

```scala
def index = <h1>Hello world</h1>
```

If the return type looks like a binary stream, **Play** will automatically render the response as binary. So generating
a captcha image using the built-in `captcha` helper can be written as:

```scala
def index = Images.captcha
```

### Controller interceptors
Controller interceptors work almost the same way as for a **Java** controller. We simply have to annotate any controller
method with the corresponding interceptor annotation:

```scala
@Before def logRequests {
  println("New request...")
```

Here the `logRequests` method does not return any value. So the request execution will continue by invoking the next
interceptors and eventually the action method.

But we can also write some interceptor that returns a value:

```scala
@Before def protectActions = {
  Forbidden
}
```

Here the execution will stop, and the `Forbidden` value will be used to generate the **HTTP** response.

If we want to continue the request execution, we can just make our interceptor return `Continue`:

```scala
@Before def protectActions = {
  session("isAdmin") match {
    case Some("yes") => Continue
    case _ => Forbidden("Restricted to administrators")
  }
}
```

### Mixing controllers using Traits
**Scala Traits** can be used to compose controllers more efficiently by mixing several aspects. We can define both
action methods and interceptors in a controller **Trait**.

For example, the following `Secure` trait adds a security interceptor to any controller applying the **Trait**:

```scala
trait Secure {
  self: Controller =>

  @Before checkSecurity = {
    session("username") match {
      case Some(username) => renderArgs += "user" -> User(username)
                             Continue
      case None => Action(Authentication.login)
    }
  }

  def connectedUser = renderArgs("user").get
}
```

And we can use it to create a secured controller:

```scala
object Application extends Controller with Secure {
  def index = <h1>Hello { connectedUser.name }!</h1>
}
```

<small>Here we used the `self: Controller =>` notation to indicate that this **Trait** can only be mixed with a
`Controller` type.</small>

### Example controller
```scala
package controllers

import javax.inject._

import play.api.mvc._
import play.api.i18n._

@Singleton
class TaskList1 @Inject()(cc: ControllerComponents) extends AbstractController(cc) {
  def taskList = TODO
  // or
  def taskList = Action {
    Ok("This works!")
  }
  // or
  def taskList = Action {
    val tasks = List("task1", "task2", "task3")
    Ok(views.html.taskList1(tasks))
  }
}
```

## HTTP routing
### The built-in HTTP router
The router is the component in charge of translating each incoming **HTTP** request to an `Action`.

An **HTTP** request is seen as an event by the **MVC** framework. This event contains two major pieces of information:

- the request path (e.g. `/clients/1542`, `/photos/list`), including the query string
- the **HTTP** method (e.g. `GET`, `POST`, ...)

Routes are defined in the `conf/routes` file, which is compiled. This means that we'll see route errors directly in our
browser:

![Route Compilation
Error](https://www.playframework.com/documentation/2.8.x/resources/manual/working/scalaGuide/main/http/images/routesError.png)

### Dependency Injection
**Play's** default routes generator creates a router class that accepts controller instances in an `@Inject`-annotated
constructor. That means the class is suitable for use with dependency injection and can also be instantiated manually
using the constructor.

Before **Play 2.7.0**, **Play** supported a static routes generator that allowed defining controllers as `object`s
instead of `class`es. That is no longer supported, as **Play** no longer relies on static state. If we wish to use our
own static state we can still do so in a controller that is a `class`.

### The routes file syntax
`conf/routes` is the configuration file used by the router. This file lists all of the routes needed by the application.
Each route consists of an **HTTP** method an **URI** pattern, both associated with a call to an `Action` generator.

Here is what a route definition looks like:

```ini
GET   /clients/:id          controllers.Clients.show(id: Long)
```

Each route starts with the **HTTP** method, followed by the **URI** pattern. The last element is the call definition.

We can also add comments to the route file, with the `#` character.

```ini
# Display a client.
GET   /clients/:id          controllers.Clients.show(id: Long)
```

We can tell the routes file to use a different router under a specific prefix by using `->` followed by the given
prefix:

```ini
->      /api                        api.MyRouter
```

This is especially useful when combined with [**String Interpolating Routing
DSL**](https://www.playframework.com/documentation/2.8.x/ScalaSirdRouter) also known as **SIRD** routing, or when
working with sub projects that route using several routes files.

It is also possible to apply modifiers by preceding the route with a line starting with a `+`. This can change the
behaviour of certain **Play** components. One such modifier is the `nocsrf` modifier to bypass the **CSRF** filter:

```ini
+ nocsrf
POST  /api/new              controllers.Api.newThing
```

### The HTTP method
The **HTTP** method can be any of the valid methods supported by **HTTP** (`GET`, `PATCH`, `POST`, `PUT`, `DELETE`,
`HEAD`).

### The URI pattern
The **URI** pattern defines the route's request path. Parts of the request path can be dynamic.

#### Static path
For example, to exactly match incoming `GET /clients/all` requests, we can define this route:

```ini
GET   /clients/all          controllers.Clients.list()
```

#### Dynamic parts
If we want to define a route that retrieves a client by **ID**, we'll need to add a dynamic part:

```ini
GET   /clients/:id          controllers.Clients.show(id: Long)
```

<small>**Note:** A **URI** pattern may have more than one dynamic part.</small>

The default matching strategy for a dynamic part is defined by the regular expression `[^/]+`, meaning that any dynamic
part defined as `:id` will match exactly one **URI** path segment. Unlike other pattern types, path segments are
automatically **URI**-decoded in the route, before being passed to our controller, and encoded in the reverse route.

#### Dynamic parts spanning several `/`
If we want a dynamic part to capture more than one **URI** path segment, separated by forward slashes, we can define a
dynamic part using the `*id` syntax, also known as a wildcard pattern, which uses the `.*` regular expression:

```ini
GET   /files/*name          controllers.Application.download(name)
```

Here for a request like `GET /files/images/logo.png`, the `name` dynamic part will capture the `images/logo.png` value.

Dynamic parts spanning several `/` are not decoded by the router or encoded by the reverse router. It is our
responsibility to validate the raw **URI** segment as we would for any user input. The reverse router simply does a
string concatenation, so we will need to make sure the resulting path is valid, and does not, for example, contain
multiple leading slashes or non-ASCII characters.

#### Dynamic parts with custom regular expressions
We can also define our own regular expression for the dynamic part, using the `$id<regex>` syntax:

```ini
GET   /items/$id<[0-9]+>    controllers.Items.show(id: Long)
```

Just like with wildcard routes, the parameter is not decoded by the router or encoded by the reverse router. We're
responsible for validating the input to make sure it makes sense in that context.

### Call to the Action generator method
The last part of a route definition is the call. This part must define a valid call to a method returning a
`play.api.mvc.Action` value, which will typically be a controller action method.

If the method does not define any parameters, we just give the fully-qualified method name:

```ini
GET   /                     controllers.Application.homePage()
```

If the action method defines some parameters, all these parameter values will be searched for in the request **URI**,
either extracted from the **URI** path itself, or from the query string.

```ini
# Extract the page parameter from the path.
GET   /:page                controllers.Application.show(page)
```

Or:

```ini
# Extract the page parameter from the query string.
GET   /                     controllers.Application.show(page)
```

Here is the corresponding `show` method definition in the `controllers.Application` controller:

```scala
def show(page: String) = Action {
  loadContentFromDatabase(page)
    .map { htmlContent =>
      Ok(htmlContent).as("text/html")
    }
    .getOrElse(NotFound)
}
```

#### Parameter types
For parameters of type `String`, typing the parameter is optional. If we want **Play** to transform the incoming
parameter into a specific **Scala** type, we can explicitly type the parameter:

```ini
GET   /clients/:id          controllers.Clients.show(id: Long)
```

And do the same on the corresponding `show` method definition in the `controllers.Clients` controller:

```scala
def show(id: Long) = Action {
  Client
    .findById(id)
    .map { client =>
      Ok(views.html.Clients.display(client))
    }
    .getOrElse(NotFound)
}
```

**Play** supports the following **Parameter Types**:

- `String`
- `Int`
- `Long`
- `Double`
- `Float`
- `Boolean`
- `UUID`
- `AnyVal` wrappers for other supported types

If we have a different type and want to implement it we can use [**Request
Binders**](https://www.playframework.com/documentation/2.8.x/ScalaRequestBinders).

#### Parameters with fixed values
Sometimes we'll want to use a fixed value for a parameter:

```ini
# Extract the page parameter from the path, or fix the value for /
GET   /                     controllers.Application.show(page = "home")
GET   /:page                controllers.Application.show(page)
```

#### Parameters with default values
We can also provide a default value that will be used if no value is found in the incoming request:

```ini
# Pagination links, like /clients?page=3
GET   /clients              controllers.Clients.list(page: Int ?= 1)
```

#### Optional parameters
We can also specify an optional parameter that does not need to be present in all requests:

```ini
# The version parameter is optional. E.g. /api/list-all?version=3.0
GET   /api/list-all         controllers.Api.list(version: Option[String])
```

#### List parameters
We can also specify list parameters for repeated query string parameters:

```ini
# The item parameter is a list.
# E.g. /api/list-items?item=red&item=new&item=slippers
GET   /api/list-items      controllers.Api.listItems(item: List[String])
# or
# E.g. /api/list-int-items?item=1&item=42
GET   /api/list-int-items  controllers.Api.listIntItems(item: List[Int])
```

### Routing priority
Many routes can match the same request. If there is a conflict, the first route (in declaration order) is used.

### Reverse routing
The router can also be used to generate a **URL** from within a **Scala** call. This makes it possible to centralize all
our **URI** patterns in a single configuration file, so we can be more confident when refactoring our application.

For each controller used in the routes file, the router will generate a *'reverse controller'* in the `routes` package,
having the same action methods, with the same signature, but returning a `play.api.mvc.Call` instead of a
`play.api.mvc.Action`.

The `play.api.mvc.Call` defines an **HTTP** call, and provides both the **HTTP** method and the **URI**.

For example, if we create a controller like:

```scala
package controllers
  import javax.inject.Inject

  import play.api._
  import play.api.mvc._

  class Application @Inject() (cc: ControllerComponents) extends AbstractController(cc) {
    def hello(name: String) = Action {
      Ok("Hello " + name + "!")
    }
  }
```

And if we map it in the `conf/routes` file:

```ini
# Hello action
GET   /hello/:name          controllers.Application.hello(name)
```

We can then reverse the **URL** to the `hello` action method, by using the `controllers.routes.Application` reverse
controller:

```scala
// Redirect to /hello/Bob
def helloBob = Action {
  Redirect(routes.Application.hello("Bob"))
}
```

<small>**Note:** There is a `routes` subpackage for each controller package. So the action
`controllers.Application.hello` can be reversed via `controllers.routes.Application.hello` (as long as there is no other
route before it in the routes file that happens to match the generated path).</small>

The reverse action method works quite simply: it takes our parameters and substitutes them back into the route pattern.
In the case of path segments (`:foo`), the value is encoded before the substitution is done. For regex and wildcard
patterns the string is substituted in raw form, since the value may span multiple segments. We must make sure we escape
those components as desired when passing them to the reverse route, and avoid passing unvalidated user input.

### Relative routes
There are instances where returning a relative route instead of an absolute may be useful. The routes returned by
`play.mvc.Call` are always absolute (they lead with a `/`), which can lead to problems when requests to our web
application are rewritten by **HTTP** proxies, load balancers, and **API** gateways. Some examples where using a
relative route would be useful include:

- Hosting an app behind a web gateway that prefixes all routes with something other than what is configured in our
`conf/routes` file, and roots our application at a route it's not expecting.

- When dynamically rendering stylesheets, we need asset links to be relative because they may end up getting served from
different **URLs** by a **CDN**.

To be able to generate a relative route we need to know what to make the target route relative to (the start route). The
start route can be retrieved from the current `RequestHeader`. Therefore, to generate a relative route it's required
that we pass in our current `RequestHeader` or the start route as a `String` parameter.

For example, given controller endpoints like:

```scala
package controllers

import javax.inject._
import play.api.mvc._

@Singleton
class Relative @Inject() (cc: ControllerComponents) extends AbstractController(cc) {
  def helloview() = Action { implicit request =>
    Ok(views.html.hello("Bob"))
  }

  def hello(name: String) = Action {
    Ok(s"Hello $name!")
  }
}
```

<small>**Note:** The current request is passed to the view template implicitly by declaring an `implicit
request`.</small>

And if we map it in the `conf/routes` file:

```ini
GET     /foo/bar/hello              controllers.Relative.helloview
GET     /hello/:name                controllers.Relative.hello(name)
```

We can then define relative routes using the reverse router as before and include an additional call to `relative`:

```scala
@(name: String)(implicit request: RequestHeader)

<h1>Hello @name</h1>

<a href="@routes.Relative.hello(name)">Absolute Link</a>
<a href="@routes.Relative.hello(name).relative">Relative Link</a>
```

<small>**Note:** The `Request` passed from the controller is cast to a `RequestHeader` and is marked `implicit` in the
view parameters. It is then passed implicitly to the call to `relative`.</small>

When requesting `/foo/bar/hello` the generated **HTML** will look like so:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bob</title>
</head>
<body>
  <a href="/hello/Bob">Absolute Link</a>
  <a href="../../hello/Bob">Relative Link</a>
</body>
</html>
```

### The Default Controller
**Play** includes a `Default` controller which provides a handful of useful actions. These can be invoked directly from
the routes file:

```ini
# Redirects to https://www.playframework.com/ with 303 See Other
GET   /about      controllers.Default.redirect(to = "https://www.playframework.com/")

# Responds with 404 Not Found
GET   /orders     controllers.Default.notFound

# Responds with 500 Internal Server Error
GET   /clients    controllers.Default.error

# Responds with 501 Not Implemented
GET   /posts      controllers.Default.todo
```

In this example, `GET /about` redirects to an external website, but it's also possible to redirect to another action
(such as `/posts` in the above example).

### Custom routing
**Play** provides a **DSL** for defining embedded routers called the **String Interpolating Routing DSL**, or **SIRD**
for short. This **DSL** has many uses, including embedding a light weight **Play** server, providing custom or more
advanced routing capabilities to a regular **Play** application, and mocking **REST** services for testing.

## The template engine
### A type safe template engine based on Scala
**Play** comes with [**Twirl**](https://github.com/playframework/twirl), a powerful **Scala**-based template engine,
whose design was inspired by **ASP.NET Razor**. Specifically it is:

- **compact, expressive and fluid:** it minimizes the number of characters and keystrokes required in a file, and
  enables a fast, fluid coding workflow. Unlike most template syntaxes, we do not need to interrupt our coding to
  explicitly denote server blocks within our **HTML**. The parser is smart enough to infer this from our code. This
  enables a really compact and expressive syntax which is clean, fast and fun to type.

- **easy to learn:** it allows us to quickly become productive, with a minimum of concepts. We use simple **Scala**
  constructs and all our existing **HTML** skills.

- **not a new language:** it was a conscious decision not to create a new language. Instead the goal was to enable
  **Scala** developers to use their existing **Scala** language skills, and deliver a template markup syntax that
  enables an awesome **HTML** construction workflow.

- **editable in any text editor:** it doesn't require a specific tool and enables us to be productive in any plain old
  text editor.

Templates are compiled, so we will see any errors in our browser:

![Twirl
Error](https://www.playframework.com/documentation/2.8.x/resources/manual/working/scalaGuide/main/templates/images/templatesError.png)

### Overview
A **Play Scala** template is a simple text file that contains small blocks of **Scala** code. Templates can generate any
text-based format, such as **HTML**, **XML**, or **CSV**.

The template system has been designed to feel comfortable to those used to working with **HTML**, allowing front-end
developers to easily work with the templates.

Templates are compiled as standard **Scala** functions, following a simple naming convention. If we create a
`views/Application/index.scala.html` template file, it will generate a `views.html.Application.index` class that has an
`apply()` method.

For example, here is a simple template:

```scala
@(customer: Customer, orders: List[Order])

<h1>Welcome @customer.name!</h1>

<ul>
@for(order <- orders) {
  <li>@order.title</li>
}
</ul>
```

We can then call this from **Scala** code as we would normally call a method on a class:

```scala
val content = views.html.Application.index(c, o)
```

### Syntax: the magic `@` character
The **Scala** template uses `@` as the single special character. Every time this character is encountered, it indicates
the beginning of a dynamic statement. We are not required to explicitly close the code block - the end of the dynamic
statement will be inferred from our code:

```scala
Hello @customer.name!
       ^^^^^^^^^^^^^
       Dynamic code
```

Because the template engine automatically detects the end of our code block by analysing our code, this syntax only
supports simple statements. If we want to insert a multi-token statement, we can explicitly mark it using brackets:

```scala
Hello @(customer.firstName + customer.lastName)!
       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                    Dynamic Code
```

We can also use curly brackets, to write a multi-statement block:

```scala
Hello @{val name = customer.firstName + customer.lastName; name}!
       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                             Dynamic Code
```

Because `@` is a special character, we'll sometimes need to escape it. We do this by using `@@`:

```scala
My email is bob@@example.com
```

<small>**Note:** We must make sure not to include whitespaces between keywords of dynamic statements and parentheses.
For example, the following code doesn't work:

```scala
@for (menu <- menuList) { ... }  // Compilation error: '(' expected but ')' found.
    ^
```
</small>

### Template parameters
A template is like a function, so it needs parameters, which must be declared at the top of the template file:

```scala
@(customer: Customer, orders: List[Order])
```

We can also use default values for parameters:

```scala
@(title: String = "Home")
```

Or even several parameter groups:

```scala
@(title: String)(body: Html)
```

### Template constructor
Be default, a template is generated as a static function that can be invoked in any context. If our template has
dependencies on components, such as the messages **API**, we may find it easier to inject it with the components (and
other templates) that it needs, and then we can inject that template into the controllers that use it.

**Twirl** supports declaring a constructor for templates, using `@this()` syntax at the start of the file, before the
template parameters. The arguments to the constructor can be defined in the same way as the template parameters:

```scala
@this(myComponent: MyComponent)

@(customer: Customer, orders: List[Order])
```

### Iterating
We can use the `for` keyword, in a pretty standard way:

```scala
<ul>
@for(p <- products) {
  <li>@p.name ($@p.price)</li>
}
</ul>
```

<small>**Note:** We must make sure that the `{` is on the same line with `for` to indicate that the expression continues
to the next line.</small>

### If-blocks
If-blocks are nothing special. We can simple use **Scala's** standard `if` statement:

```scala
@if(items.isEmpty) {
  <h1>Nothing to display</h1>
} else {
  <h1>@items.size items!</h1>
}
```

### Declaring reusable blocks
We can create reusable code blocks:

```scala
@display(product: Product) = {
  @product.name ($@product.price)
}

<ul>
@for(product <- products) {
  @display(product)
}
</ul>
```

We can also declare reusable pure code blocks:

```scala
@title(text: String) = @{
  text.split(' ').map(_.capitalize).mkString(" ")
}

<h1>@title("hello world")</h1>
```

<small>**Note:** Declaring a code block this way in a template can sometimes be useful, but we should keep in mind that
a template is not the best place to write complex logic. It is often better to externalize this kind of code in a
**Scala** class (that we can store under the `views/` package as well if we want).</small>

By convention, a reusable block defined with a name starting with **implicit** will be marked as `implicit`:

```scala
@implicitFieldConstructor = @{ MyFieldConstructor() }
```

### Declaring reusable values
We can define scoped values using the `defining` helper:

```scala
@defining(user.firstName + " " + user.lastName) { fullName =>
  <div>Hello @fullName</div>
}
```

### Import statements
We can import whatever we want at the beginning of our template (or sub-template):

```scala
@(customer: Customer, orders: List[Order])

@import utils._

...
```

To make an absolute resolution, we can use the **root** prefix in the import statement.

```scala
@import _root_.company.product.core._
```

If we have common imports, which we need in all our templates, we can declare them in `build.sbt`:

```scala
TwirlKeys.templateImports += "org.abc.backend._"
```

### Comments
We can write server-side block comments in templates using `@* *@`:

```scala
@*********************
* This is a comment  *
*********************@
```

We can put a comment on the first line to document our template into the **Scala API** doc:

```scala
@**************************************
 * Home page.                         *
 *                                    *
 * @param msg The message to display  *
 *************************************@
@(msg: String)

<h1>@msg</h1>
```

### Escaping
Be default, dynamic content parts are escaped according to the template type's (e.g. **HTML** or **XML**) rules. If we
want to output a raw content fragment, we can wrap it in the template content type.

For example to output raw **HTML**:

```scala
<p>
  @Html(article.content)
</p>
```

<small>**Note:** When using this feature, we must consider the security implications of outputting raw **HTML** if there
is any possibility that the user can control the content. This technique is a common cause of **Cross Site Scripting**
(**XSS**) vulnerabilities and is dangerous if not used with caution.</small>

### String interpolation
The template engine can be used as a string interpolator. We basically trade the `@` for a `$`:

```scala
import play.twirl.api.StringInterpolation

val name = "Martin"
val p    = html"<p>Hello $name</p>"
```

### Displaying Scala types
**Twirl** typically renders values of **Scala** types by calling the `toString` method on them. However, if values are
wrapped inside `Option` or collections (`Seq`, `Array`, `TraversableOnce`), **Twirl** first unwraps the values and then
calls `toString`.

For example,

```scala
<ul>
  <li>@Option("value inside option")</li>
  <li>@List("first", "last")</li>
  <li>@User("Foo", "Bar")</li>
  <li>@List("hello", User("Foo", "Bar"), Option("value inside option"), List("first", "last"))</li>
</ul>
```

is rendered in the browser as

![Displaying Scala
Types](https://www.playframework.com/documentation/2.8.x/resources/manual/working/scalaGuide/main/templates/images/displayScalaTypes.png)

## Dependency injection with templates
**Twirl** templates can be generated as a class rather than a static object by declaring a constructor using a special
`@this(args)` syntax at the top of the template. This means that **Twirl** templates can be injected into controllers
directly and can manage their own dependencies, rather than the controller having to manage dependencies not only for
itself, but also for the templates it has to render.

As an example, suppose a template has a dependency on a component `Summarizer`, which is not used by the controller:

```scala
trait Summarizer {
  /** Provide short form of string if over a certain length */
  def summarize(item: String)
}
```

We create a file `app/views/IndexTemplate.scala.html` using the `@this` syntax for the constructor:

```scala
@this(summarizer: Summarizer)
@(item: String)

@{summarizer.summarize(item)}
```

And finally we define the controller in **Play** by injecting the template in the constructor:

```scala
public MyController @Inject()(template: views.html.IndexTemplate,
                              cc: ControllerComponents)
  extends AbstractController(cc) {

  def index = Action { implicit request =>
    val item = "some extremely long text"
    Ok(template(item))
  }
}
```

Once the template is defined with its dependencies, then the controller can have the template injected into the
controller, but the controller does not see `Summarizer`.

If we are using **Twirl** outside of a **Play** application, we will have to manually add the `@Inject` annotation
saying that dependency injection should be used here:

```scala
TwirlKeys.constructorAnnotations += "@javax.inject.Inject()"
```

Inside a **Play** application, this is already included in the default settings.

## Scala template common use cases
Templates, being simple functions, can be composed in any way we want. Below are examples of some common scenarios.

### Layout
Let's declare a `views/main.scala.html` template that will act as a main layout template:

```scala
@(title: String)(content: Html)
<!DOCTYPE html>
<html>
  <head>
    <title>@title</title>
  </head>
  <body>
    <section class="content">@content</section>
  </body>
</html>
```

As we can see, this template takes two parameters: a title and an **HTML** content block. Now we can use it from another
`views/Application/index.scala.html` template:

```scala
@main(title = "Home") {
  <h1>Home page</h1>
}
```

<small>**Note:** We sometimes use named parameters, like `@main(title = "Home")`, sometimes not, like `@main("Home")`.
We can use whatever is clearer in a specific context.</small>

Sometimes we need a second page-specific content block for a sidebar or breadcrumb trail, for example. We can do this
with an additional parameter:

```scala
@(title: String)(sidebar: Html)(content: Html)
<!DOCTYPE html>
<html>
  <head>
    <title>@title</title>
  </head>
  <body>
    <section class="sidebar">@sidebar</section>
    <section class="content">@content</section>
  </body>
</html>
```

Using this from our "index" template, we have:

```scala
@main("Home") {
  <h1>Sidebar</h1>
} {
  <h1>Home page</h1>
}
```

Alternatively, we can declare the sidebar block separately:

```scala
@sidebar = {
  <h1>Sidebar</h1>
}

@main("Home")(sidebar) {
  <h1>Home page</h1>
}
```

### Tags (they are just functions, right?)
Let's write a simple `views/tags/notice.scala.html` tag that displays an **HTML** notice:

```scala
@(level: String = "error")(body: (String) => Html)

@level match {
  case "success" => {
    <p class="success">
      @body("green")
    </p>
  }

  case "warning" => {
    <p class="warning">
      @body("orange")
    </p>
  }

  case "error" => {
    <p class="error">
      @body("red")
    </p>
  }
}
```

And now let's use it from another template:

```scala
@import tags._

@notice("error") { color =>
  Oops, something is <span style="color:@color">wrong</span>
}
```

### Includes
Again, there's nothing special here. We can just call any other template we like (and in fact any other function coming
from anywhere at all):

```scala
<h1>Home</h1>

<div id="side">
  @common.sideBar()
</div>
```

### moreScripts and moreStyles equivalents
To define old `moreScripts` and `moreStyles` variable equivalents (like on **Play! 1.x**) on a **Scala** template, we
can define a variable in the main template like this:

```scala
@(title: String, scripts: Html = Html(""))(content: Html)

<!DOCTYPE html>

<html>
<head>
  <title>@title</title>
  <link rel="stylesheet" media="screen" href="@routes.Assets.at("stylesheets/main.css")">
  <link rel="shortcut icon" type="image/png" href="@routes.Assets.at("images/favicon.png")">
  <script src="@routes.Assets.at("javascripts/jquery-1.7.1.min.js")" type="text/javascript"></script>
  @scripts
</head>
<body>
  <div class="navbar navbar-fixed-top">
    <div class="navbar-inner">
      <div class="container">
        <a class="brand" href="#">Movies</a>
      </div>
    </div>
  </div>
  <div class="container">
    @content
  </div>
</body>
</html>
```

On an extended template that needs an extra script:

```scala
@scripts = {
  <script type="text/javascript">alert("hello!");</script>
}

@main("Title",scripts){
  Html content here ...
}
```

And on an extended template that doesn't need an extra script, just like this:

```scala
@main("Title"){
  Html content here ...
}
```

## Handling form submission
### Overview
Form handling and submission is an important part of any web application. **Play** comes with features that make
handling simple forms easy and complex forms possible.

**Play's** form handling approach is based around the concept of binding data. When data comes in from a **POST**
request, **Play** will look for formatted values and bind them to a `Form` object. From there, **Play** can use the
bound form to fill a case class with data, call custom validations, and so on.

Typically forms are used directly from a `BaseController` instance. However, `Form` definitions do not have to match up
exactly with case classes or models: they are purely for handling input and it is reasonable to use a distinct `Form`
for a distinct **POST**.

### Imports
To use forms, we import the following packages into our class:

```scala
import play.api.data._
import play.api.data.Forms._
```

To make use of validation and constraints, we import the following packages into our class:

```scala
import play.api.data.validation.Constraints._
```

### Defining a form
First, we define a case class which contains the elements we want in the form. Here we want to capture the name and age
of a user, so we create a `UserData` object:

```scala
case class UserData(name: String, age: Int)
```

Now that we have a case class, the next step is to define a `Form` structure. The function of a `Form` is to transform
form data into a bound instance of a case class, and we define it like follows:

```scala
val userForm = Form(
  mapping(
    "name" -> text,
    "age"  -> number
  )(UserData.apply)(UserData.unapply)
)
```

The [**Forms**](https://www.playframework.com/documentation/2.8.x/api/scala/play/api/data/Forms$.html) object defines
the `mapping` method. This method takes the names and constraints of the form, and also takes two functions: an `apply`
function and an `unapply` function. Because `UserData` is a case class, we can plug its `apply` and `unapply` methods
directly into the mapping method.

<small>**Note:** Maximum number of fields for a single tuple or mapping is 22 due to the way form handling is
implemented. If we have more than 22 fields in our form, we should break down our forms using lists or nested values.</small>

A form will create a `UserData` instance with the bound values when given a `Map`:

```scala
val anyData  = Map("name" -> "bob", "age" -> "21")
val userData = userForm.bind(anyData).get
```

But most of the time we'll use forms from within an `Action`, with data provided from the request. `Form` contains
`bindFromRequest`, which will take a request as an implicit parameter. If we define an implicit request, then
`bindFromRequest` will find it.

```scala
val userData = userForm.bindFromRequest.get
```

<small>**Note:** There is a catch to using `get` here. If the form cannot bind to the data, then `get` will throw an
exception. We'll see a safer way of dealing with input in the next few sections.</small>

We are not limited to using case classes in our form mapping. As long as the apply and unapply methods are properly
mapped, we can pass in anything we like, such as tuples using the `Forms.tuple` mapping or model case classes. However,
there are several advantages to defining a case class specifically for a form:

- **Form specific case classes are convenient.** Case classes are designed to be simple containers of data, and provide
  out of the box features that are a natural match with `Form` functionality.

- **Form specific case classes are powerful.** Tuples are convenient to use, but do not allow for custom apply or
  unapply methods, and can only reference contained data by arity (`_1`, `_2`, etc.).

- **Form specific case classes are targeted specifically to the Form.** Reusing model case classes can be convenient,
  but often models will contain additional domain logic and even persistence details that can lead to tight coupling. In
  addition, if there is not a direct *1:1* mapping between the form and the model, then sensitive fields must be
  explicitly ignored to prevent a [**parameter tampering**](https://www.owasp.org/index.php/Web_Parameter_Tampering)
  attack.

### Defining constraints on the form
The `text` constraint considers empty strings to be valid. This means that `name` could be empty here without an error,
which is not what we want. A way to ensure that `name` has the appropriate value is to use the `nonEmptyText`
constraint.

```scala
val userFormConstraints2 = Form(
  mapping(
    "name" -> nonEmptyText,
    "age"  -> number(min = 0, max = 100)
  )(UserData.apply)(UserData.unapply)
)
```

Using this form will result in a form with errors if the input to the form does not match the constraints:

```scala
val boundForm = userFormConstraints2.bind(Map("bob" -> "", "age" -> "25"))
boundForm.hasErrors must beTrue
```

These out of the box constraints are defined on the `Forms` object:

- `text` - maps to `scala.String`, optionally takes `minLength` and `maxLength`
- `nonEmptyText` - maps to `scala.String`, optionally takes `minLength` and `maxLength`
- `number` - maps to `scala.Int`, optionally takes `min`, `max`, and `strict`
- `longNumber` - maps to `scala.Long`, optionally takes `min`, `max`, and `strict`
- `bigDecimal` - takes `precision` and `scale`
- `date`, `sqlDate` - maps to `java.util.Date`, `java.sql.Date`, optionally takes `pattern` and `timeZone`
- `email` - maps to `scala.String`, using an email regular expression
- `boolean` - maps to `scala.Boolean`
- `checked` - maps to `scala.Boolean`
- `optional` - maps to `scala.Option`

### Defining ad-hoc constraints
We can define our own ad-hoc constraints on the case classes using the [**validation
package**](https://www.playframework.com/documentation/2.8.x/api/scala/play/api/data/validation/index.html):

```scala
val userFormConstraints = Form(
  mapping(
    "name" -> text.verifying(nonEmpty),
    "age"  -> number.verifying(min(0), max(100))
  )(UserData.apply)(UserData.unapply)
)
```

We can also define ad-hoc constraints on the case classes themselves:

```scala
def validate(name: String, age: Int) = {
  name match {
    case "bob" if age >= 18 =>
      Some(UserData(name, age))
    case "admin" =>
      Some(UserData(name, age))
    case _ =>
      None
  }
}

val userFormConstraintsAdHoc = Form(
  mapping(
    "name" -> text,
    "age"  -> number
  )(UserData.apply)(UserData.unapply).verifying(
    "Failed form constraints!",
    fields =>
      fields match {
        case userData => validate(userData.name, userData.age).isDefined
      }
  )
)
```

### Validating a form in an Action
Now that we have constraints, we can validate the form inside an action, and process the form with errors.

We do this using the `fold` method, which takes two functions: the first is called if the binding fails, and the second
is called if the binding succeeds.

```scala
userForm.bindFromRequest.fold(
  formWithErrors => {
    // binding failure, you retrieve the form containing errors:
    BadRequest(views.html.user(formWithErrors))
  },
  userData => {
    /* binding success, you get the actual value. */
    val newUser = models.User(userData.name, userData.age)
    val id      = models.User.create(newUser)
    Redirect(routes.Application.home(id))
  }
)
```

In the failure case, we render the page with `BadRequest`, and pass in the `formWithErrors` as a parameter to the page.
If we use the view helpers (discussed below), then any errors that are bound to a field will be rendered in the page
next to the field.

In the success case, we're sending a `Redirect` with a route to `routes.Application.home` here instead of rendering a
view template. This pattern is called [**Redirect after POST**](https://en.wikipedia.org/wiki/Post/Redirect/Get), and is
an excellent way to prevent duplicate form submissions.

<small>**Note:** "Redirect with POST" is **required** when using `flashing` or other methods with [flash
scope](#session-and-flash-scopes), as new cookies will only be available after the redirected **HTTP** request.</small>

Alternatively, we can use the `parse.form` body parser that binds the content of the request to our form.

```scala
val userPost = Action(parse.form(userForm)) { implicit request =>
  val userData = request.body
  val newUser  = models.User(userData.name, userData.age)
  val id       = models.User.create(newUser)
  Redirect(routes.Application.home(id))
}
```

In the failure case, the default behaviour is to return an empty `BadRequest` response. We can override this behaviour
with our own logic. For instance, the following code is completely equivalent to the preceding one using
`bindFromRequest` and `fold`.

```scala
val userPostWithErrors = Action(
  parse.form(
    userForm,
    onErrors = (formWithErrors: Form[UserData]) => {
      implicit val messages = messagesApi.preferred(Seq(Lang.defaultLang))
      BadRequest(views.html.user(formWithErrors))
    }
  )
) { implicit request =>
  val userData = request.body
  val newUser  = models.User(userData.name, userData.age)
  val id       = models.User.create(newUser)
  Redirect(routes.Application.home(id))
}
```

### Showing forms in a view template
Once we have a form, then we need to make it available to the template engine. We do this by including the form as a
parameter to the view template. For `user.scala.html`, the header at the top of the page will look like this:

```scala
@(userForm: Form[UserData])(implicit messages: Messages)
```

Because `user.scala.html` needs a form passed in, we should pass the empty `userForm` initially when rendering
`user.scala.html`:

```scala
def index = Action { implicit request =>
  Ok(views.html.user(userForm))
}
```

The first thing is to be able to create the [**form
tag**](https://www.playframework.com/documentation/2.8.x/api/scala/views/html/helper/form$.html). It is a simple view
helper that creates a `form` tag and sets the `action` and `method` tag parameters according to the reverse route we
pass in:

```scala
@helper.form(action = routes.Application.userPost) {
  @helper.inputText(userForm("name"))
  @helper.inputText(userForm("age"))
}
```

We can find several input helpers in the `views.html.helper` package. We feed them with a form field, and they display
the corresponding **HTML** input, setting the value, constraints and displaying errors when a form binding fails.

<small>**Note:** We can use `@import helper._` in the template to avoid prefixing helpers with `@helper`.</small>

There are several input helpers, but the most helpful are:

- `form` - renders a `<form>` element
- `inputText` - renders a `<input type="text">` element
- `inputPassword` - renders a `<input type="password">` element
- `inputDate` - renders a `<input type="date">` element
- `inputFile` - renders a `<input type="file">` element
- `inputRadioGroup` - renders a `<input type="radio">` element
- `checkbox` - renders a `<input type="checkbox">` element
- `input` - renders a generic `<input>` element (which requires explicit arguments)
- `select` - renders a `<select>` element
- `textarea` - renders a `<textarea>` element

<small>**Note:** The source code for each of these templates is defined as **Twirl** templates under the `views/helper`
package, and so the packaged version corresponds to the generated **Scala** source code. For reference, it can be useful
to see the
[**views/helper**](https://github.com/playframework/playframework/tree/main/core/play/src/main/scala/views/helper)
package on **GitHub**.</small>

As with the `form` helper, we can specify an extra set of parameters that will be added to the generated **Html**:

```scala
@helper.inputText(userForm("name"), Symbol("id") -> "name", Symbol("size") -> 30)
```

The generic `input` helper mentioned above will let us code the desired **HTML** result:

```scala
@helper.input(userForm("name")) { (id, name, value, args) =>
  <input type="text" name="@name" id="@id" @toHtmlArgs(args)>
}
```

<small>**Note:** All extra parameters will be added to the generated **Html**, unless they start with the `_` character.
Arguments starting with `_` are reserved for [field constructor
arguments](#custom-field-constructors).</small>

For complex form elements, we can also create our own custom view helpers (using **Scala** classes in the `views`
package) and custom field constructors.

### Passing MessagesProvider to Form Helpers
The form helpers above--`input`, `checkbox`, and so on--all take `MessagesProvider` as an implicit parameter. The form
handlers need to take `MessagesProvider` because they need to provide error messages mapped to the language defined in
the request.

There are two ways to pass in the `MessagesProvider` object required.

#### Option One: Implicitly Convert Request to Messages
The first way is to make the controller extend `play.api.i18n.I18nSupport`, which makes use of an injected
`MessagesApi`, and will implicitly convert an implicit request to an implicit `Messages`:

```scala
class MessagesController @Inject() (cc: ControllerComponents)
    extends AbstractController(cc)
    with play.api.i18n.I18nSupport {
  import play.api.data.Form
  import play.api.data.Forms._

  val userForm = Form(
    mapping(
      "name" -> text,
      "age"  -> number
    )(views.html.UserData.apply)(views.html.UserData.unapply)
  )

  def index = Action { implicit request =>
    Ok(views.html.user(userForm))
  }
}
```

This means that the following form template will be resolved:

```scala
@(userForm: Form[UserData])(implicit request: RequestHeader, messagesProvider: MessagesProvider)

@import helper._

@helper.form(action = routes.FormController.post) {
@CSRF.formField                     @* <- takes a RequestHeader    *@
@helper.inputText(userForm("name")) @* <- takes a MessagesProvider *@
@helper.inputText(userForm("age"))  @* <- takes a MessagesProvider *@
}
```

#### Option Two: Use MessagesRequest
The second way is to dependency inject a `MessagesActionBuilder`, which provides a `MessagesRequest`:

```scala
// Example form injecting a messagesAction
class FormController @Inject() (messagesAction: MessagesActionBuilder, components: ControllerComponents)
    extends AbstractController(components) {
  import play.api.data.Form
  import play.api.data.Forms._

  val userForm = Form(
    mapping(
      "name" -> text,
      "age"  -> number
    )(views.html.UserData.apply)(views.html.UserData.unapply)
  )

  def index = messagesAction { implicit request: MessagesRequest[AnyContent] =>
    Ok(views.html.messages(userForm))
  }

  def post = TODO
}
```

This is useful because to use **CSRF** with forms, both a `Request` (technically a `RequestHeader`) and a `Messages`
object must be available to the template. By using a `MessagesRequest`, which is a `WrappedRequest` that extends
`MessagesProvider`, only a single implicit parameter needs to be made available to templates.

Because we typically don't need the body of the request, we can pass `MessagesRequestHeader`, rather than typing
`MessagesRequest[_]`:

```scala
@(userForm: Form[UserData])(implicit request: MessagesRequestHeader)

@import helper._

@helper.form(action = routes.FormController.post) {
  @CSRF.formField                     @* <- takes a RequestHeader    *@
  @helper.inputText(userForm("name")) @* <- takes a MessagesProvider *@
  @helper.inputText(userForm("age"))  @* <- takes a MessagesProvider *@
}
```

Rather than injecting `MessagesActionBuilder` into our controller, we can also make `MessagesActionBuilder` be the
default `Action` by extending `MessagesAbstractController` to incorporate form processing into our controllers.

```scala
// Form with Action extending MessagesAbstractController
class MessagesFormController @Inject() (components: MessagesControllerComponents)
    extends MessagesAbstractController(components) {
  import play.api.data.Form
  import play.api.data.Forms._

  val userForm = Form(
    mapping(
      "name" -> text,
      "age"  -> number
    )(views.html.UserData.apply)(views.html.UserData.unapply)
  )

  def index = Action { implicit request: MessagesRequest[AnyContent] =>
    Ok(views.html.messages(userForm))
  }

  def post() = TODO
}
```

### Displaying errors in a view template
The errors in a form take the form of `Map[String, FormError]` where `FormError` has:

- `key` - should be the same as the field
- `message` - a message or a message key
- `args` - a list of arguments to the message

The form errors are accessed on the bound form instance as follows:

- `errors` - returns all errors as `Seq[FormError]`
- `globalErrors` - returns errors without a key as `Seq[FormError]`
- `error("name")` - returns the first error bound to key as `Option[FormError]`
- `errors("name")` - returns all errors bound to key as `Seq[FormError]`

Errors attached to a field will render automatically using the form helpers, so `@helper.inputText` with errors can
display as follows:

```html
<dl class="error" id="age_field">
  <dt><label for="age">Age:</label></dt>
  <dd><input type="text" name="age" id="age" value=""></dd>
  <dd class="error">This field is required!</dd>
  <dd class="error">Another error</dd>
  <dd class="info">Required</dd>
  <dd class="info">Another constraint</dd>
</dl>
```

Errors that are not attached to a field can be converted to a string with `error.format`, which takes an implicit
`play.api.i18n.Messages` instance.

Global errors that are not bound to a key do not have a helper and must be defined explicitly in the page:

```scala
@if(userForm.hasGlobalErrors) {
  <ul>
  @for(error <- userForm.globalErrors) {
    <li>@error.format</li>
  }
  </ul>
}
```

### Mapping with tuples
We can use tuples instead of case classes in our fields:

```scala
val userFormTuple = Form(
  tuple(
    "name" -> text,
    "age"  -> number
  ) // tuples come with built-in apply/unapply
)
```

Using a tuple can be more convenient than defining a case class, especially for low arity tuples:

```scala
val anyData     = Map("name" -> "bob", "age" -> "25")
val (name, age) = userFormTuple.bind(anyData).get
```

### Mapping with single
Tuples are only possible when there are multiple values. If there is only one field in the form, we should use
`Forms.single` to map to a single value without the overhead of a case class or tuple:

```scala
val singleForm = Form(
  single(
    "email" -> email
  )
)

val emailValue = singleForm.bind(Map("email" -> "bob@example.com")).get
```

### Fill values
Sometimes we'll want to populate a form with existing values, typically for editing data:

```scala
val filledForm = userForm.fill(UserData("Bob", 18))
```

When we use this with a view helper, the value of the element will be filled with the value:

```scala
@helper.inputText(filledForm("name")) @* will render value="Bob" *@
```

Fill is especially helpful for helpers that need lists or maps of values, such as the `select` and `inputRadioGroup`
helpers. We should use `options` to value these helpers with lists, maps and pairs.

A single valued form mapping can set the selected options in a select dropdown:

```scala
val addressSelectForm: Form[AddressData] = Form(
  mapping(
    "street" -> text,
    "city"   -> text
  )(AddressData.apply)(AddressData.unapply)
)
```

```scala
val selectedFormValues = AddressData(street = "Main St", city = "London")
val filledForm         = addressSelectForm.fill(selectedFormValues)
```

And when this is used in a template that sets the options to a list of pairs:

```scala
@(
  addressData: Form[AddressData],
  cityOptions: List[(String, String)] = List("New York" -> "U.S. Office", "London" -> "U.K. Office", "Brussels" -> "E.U. Office")
)(implicit messages: Messages)
```

```scala
@helper.select(addressData("city"), options = cityOptions) @* Will render the selected city to be the filled value *@
@helper.inputText(addressData("street"))
```

The filled value will be selected in the dropdown based on the first value of the pair. In this case, the U.K. Office
will be displayed in the select and the option's value will be London.

### Nested values
A form mapping can define nested values by using `Forms.mapping` inside an existing mapping:

```scala
case class AddressData(street: String, city: String)

case class UserAddressData(name: String, address: AddressData)
```

```scala
val userFormNested: Form[UserAddressData] = Form(
  mapping(
    "name" -> text,
    "address" -> mapping(
      "street" -> text,
      "city"   -> text
    )(AddressData.apply)(AddressData.unapply)
  )(UserAddressData.apply)(UserAddressData.unapply)
)
```

<small>**Note:** When we are using nested data this way, the form values sent by the browser must be named like
`address.street`, `address.city`, etc.</small>

```scala
@helper.inputText(userFormNested("name"))
@helper.inputText(userFormNested("address.street"))
@helper.inputText(userFormNested("address.city"))
```

### Repeated values
A form mapping can define repeated values using `Forms.list` or `Forms.seq`:

```scala
case class UserListData(name: String, emails: List[String])
```

```scala
val userFormRepeated = Form(
  mapping(
    "name"   -> text,
    "emails" -> list(email)
  )(UserListData.apply)(UserListData.unapply)
)
```

When we are using repeated data like this, there are two alternatives for sending the form values in the **HTTP**
request. First, we can suffix the parameter with an empty bracket pair, as in "emails[]". This parameter can then be
repeated in the standard way, as in `http://foo.com/request?emails[]=a@b.com&emails[]=c@d.com`. Alternatively, the
client can explicitly name the parameters uniquely with array subscripts, as in `emails[0]`, `emails[1]`, `emails[2]`,
and so on. This approach also allows us to maintain the order of a sequence of inputs.

If we are using **Play** to generate our form **HTML**, we can generate as many inputs for the `emails` field as the
form contains, using the `repeat` helper:

```scala
@helper.inputText(myForm("name"))
@helper.repeat(myForm("emails"), min = 1) { emailField =>
  @helper.inputText(emailField)
}
```

The `min` parameter allows us to display a minimum number of fields even if the corrosponding form data is empty.

If we want to access the index of the fields we can use the `repeatWithIndex` helper instead:

```scala
@helper.repeatWithIndex(myForm("emails"), min = 1) { (emailField, index) =>
  @helper.inputText(emailField, Symbol("_label") -> ("email #" + index))
}
```

### Optional values
A form mapping can also define optional values using `Forms.optional`:

```scala
case class UserOptionalData(name: String, email: Option[String])
```

```scala
val userFormOptional = Form(
  mapping(
    "name"  -> text,
    "email" -> optional(email)
  )(UserOptionalData.apply)(UserOptionalData.unapply)
)
```

This maps to an `Option[A]` in the output, which is `None` if no form value is found.

### Default values
We can populate a form with initial values using `Form#fill`:

```scala
val filledForm = userForm.fill(UserData("Bob", 18))
```

Or we can define a default mapping on the number using `Forms.default`:

```scala
Form(
  mapping(
    "name" -> default(text, "Bob"),
    "age"  -> default(number, 18)
  )(UserData.apply)(UserData.unapply)
)
```

Default values are used only when:

1. Populating the `Form` from data, for example, from the request
2. And there is no corresponding data for the field

The default value is not used when creating the form.

### Ignored values
If we want a form to have a static value for a field, we should use `Forms.ignored`:

```scala
val userFormStatic = Form(
  mapping(
    "id"    -> ignored(23L),
    "name"  -> text,
    "email" -> optional(email)
  )(UserStaticData.apply)(UserStaticData.unapply)
)
```

### Custom binders for form mappings
Each form mapping uses an implicitly provided `Formatter[T]` binder object that performs the conversion of incoming
`String` form data to/from the target data type.

```scala
case class UserCustomData(name: String, website: java.net.URL)
```

To bind to a custom type like `java.net.URL` in the example above, we must define a form mapping like this:

```scala
val userFormCustom = Form(
  mapping(
    "name"    -> text,
    "website" -> of[URL]
  )(UserCustomData.apply)(UserCustomData.unapply)
)
```

For this to work we will need to make an implicit `Formatter[java.net.URL]` available to perform the data
binding/unbinding.

```scala
import play.api.data.format.Formatter
import play.api.data.format.Formats._
implicit object UrlFormatter extends Formatter[URL] {
  override val format                                       = Some(("format.url", Nil))
  override def bind(key: String, data: Map[String, String]) = parsing(new URL(_), "error.url", Nil)(key, data)
  override def unbind(key: String, value: URL)              = Map(key -> value.toString)
}
```

The `Formats.parsing` function is used to capture any exceptions thrown in the act of converting a `String` to target
type `T` and registers a `FormError` on the form field binding.

### Putting it all together
Here's an example of what a model and controller would look like for managing an entity.

Given the case class `Contact`:

```scala
case class Contact(
  firstname: String,
  lastname: String,
  company: Option[String],
  informations: Seq[ContactInformation]
)

object Contact {
  def save(contact: Contact): Int = 99
}

case class ContactInformation(label: String, email: Option[String], phones: List[String])
```

`Contact` contains a `Seq` with `ContactInformation` elements and a `List` of `String`. In this case, we can combine the
nested mapping with repeated mappings (defined with `Forms.seq` and `Forms.list`, respectively).

```scala
val contactForm: Form[Contact] = Form(
  // Defines a mapping that will handle Contact values
  mapping(
    "firstname" -> nonEmptyText,
    "lastname"  -> nonEmptyText,
    "company"   -> optional(text),
    // Defines a repeated mapping
    "informations" -> seq(
      mapping(
        "label" -> nonEmptyText,
        "email" -> optional(email),
        "phones" -> list(
          text.verifying(pattern("""[0-9.+]+""".r, error = "A valid phone number is required"))
        )
      )(ContactInformation.apply)(ContactInformation.unapply)
    )
  )(Contact.apply)(Contact.unapply)
)
```

And this code shows how an existing contact is displayed in the form using filled data:

```scala
def editContact = Action { implicit request =>
  val existingContact = Contact(
    "Fake",
    "Contact",
    Some("Fake company"),
    informations = List(
      ContactInformation(
        "Personal",
        Some("fakecontact@gmail.com"),
        List("01.23.45.67.89", "98.76.54.32.10")
      ),
      ContactInformation(
        "Professional",
        Some("fakecontact@company.com"),
        List("01.23.45.67.89")
      ),
      ContactInformation(
        "Previous",
        Some("fakecontact@oldcompany.com"),
        List()
      )
    )
  )
  Ok(views.html.contact.form(contactForm.fill(existingContact)))
}
```

Finally, this is what a form submission handler would look like:

```scala
def saveContact = Action { implicit request =>
  contactForm.bindFromRequest.fold(
    formWithErrors => {
      BadRequest(views.html.contact.form(formWithErrors))
    },
    contact => {
      val contactId = Contact.save(contact)
      Redirect(routes.Application.showContact(contactId)).flashing("success" -> "Contact saved!")
    }
  )
}
```

## Using custom validations
The [**validation
package**](https://www.playframework.com/documentation/2.8.x/api/scala/play/api/data/validation/index.html) allows us to
create ad-hoc constraints using the `verifying` method. However, **Play** gives us the option of creating our own custom
constraints, using the `Constraint` case class.

Here, we'll implement a simple password strength constraint that uses regular expressions to check the password is not
all letters or numbers. A `Constraint` takes a function which returns a `ValidationResult`, and we use that function to
return the results of the password check:

```scala
val allNumbers = """\d*""".r
val allLetters = """[A-Za-z]*""".r

val passwordCheckConstraint: Constraint[String] = Constraint("constraints.passwordcheck")({ plainText =>
  val errors = plainText match {
    case allNumbers() => Seq(ValidationError("Password is all numbers"))
    case allLetters() => Seq(ValidationError("Password is all letters"))
    case _            => Nil
  }
  if (errors.isEmpty) {
    Valid
  } else {
    Invalid(errors)
  }
})
```

We can then use this constraint together with `Constraints.min` to add additional checks on the password.

```scala
val passwordCheck: Mapping[String] = nonEmptyText(minLength = 10)
  .verifying(passwordCheckConstraint)
```

## Custom Field Constructors
A field rendering is not only composed of the `<input>` tag, but it also needs a `<label>` and possibly other tags used
by our **CSS** framework to decorate the field.

All input helpers take an implicit `FieldConstructor` that handles this part. The [**default
one**](https://www.playframework.com/documentation/2.8.x/api/scala/views/html/helper/defaultFieldConstructor$.html)
(used if there are no other field constructors available in the scope), generates **HTML** like:

```html
<dl class="error" id="username_field">
  <dt><label for="username">Username:</label></dt>
  <dd><input type="text" name="username" id="username" value=""></dd>
  <dd class="error">This field is required!</dd>
  <dd class="error">Another error</dd>
  <dd class="info">Required</dd>
  <dd class="info">Another constraint</dd>
</dl>
```

This default field constructor supports additional options we can pass in the input helper arguments:

```scala
'_label -> "Custom label"
'_id -> "idForTheTopDlElement"
'_help -> "Custom help"
'_showConstraints -> false
'_error -> "Force an error"
'_showErrors -> false
```

### Writing our own field constructor
Often we will need to write our own field constructor. We start by writing a template like:

```scala
@(elements: helper.FieldElements)

<div class="@if(elements.hasErrors) {error}">
  <label for="@elements.id">@elements.label</label>
  <div class="input">
    @elements.input
    <span class="errors">@elements.errors.mkString(", ")</span>
    <span class="help">@elements.infos.mkString(", ")</span>
  </div>
</div>
```

<small>**Note:** This is just a sample. We can make it as complicated as we need. We also have access to the original
field using `@elements.field`.</small>

Now we can create a `FieldConstructor` using this template function:

```scala
object MyHelpers {
  import views.html.helper.FieldConstructor
  implicit val myFields = FieldConstructor(html.myFieldConstructorTemplate.f)
}
```

And to make the form helpers use it, we just import it in our templates:

```scala
@import MyHelpers._
@helper.inputText(myForm("username"))
```

It will then use our field constructor to render the input text.

We can also set an implicit value for our `FieldConstructor` inline:

```scala
@implicitField = @{ helper.FieldConstructor(myFieldConstructorTemplate.f) }
@helper.inputText(myForm("username"))
```

## Handling file upload
### Uploading files in a form using `multipart/form-data`
The standard way to upload files in a web application is to use a form with a special `multipart/form-data` encoding,
which lets us mix standard form data with file attachment data.

<small>**Note:** The **HTTP** method used to submit the form must be `POST` (not `GET`).</small>

We start by writing an **HTML** form:

```scala
@helper.form(action = routes.HomeController.upload, Symbol("enctype") -> "multipart/form-data") {
  <input type="file" name="picture">

  <p>
    <input type="submit">
  </p>
}
```

We should add a **CSRF** token to the form, unless we have the **CSRF** filter disabled. The **CSRF** filter checks the
multi-part form in the order the fields are listed, so we must put the **CSRF** token before the file input field. This
improves efficiency and avoids a token-not-found error if the file size exceeds `play.filters.csrf.body.bufferSize`.

Now we define the `upload` action using a `multipartFormData` body parser:

```scala
def upload = Action(parse.multipartFormData) { request =>
  request.body
    .file("picture")
    .map { picture =>
      // only get the last part of the filename
      // otherwise someone can send a path like ../../home/foo/bar.txt to write to other files on the system
      val filename    = Paths.get(picture.filename).getFileName
      val fileSize    = picture.fileSize
      val contentType = picture.contentType

      picture.ref.copyTo(Paths.get(s"/tmp/picture/$filename"), replace = true)
      Ok("File uploaded")
    }
    .getOrElse {
      Redirect(routes.HomeController.index).flashing("error" -> "Missing file")
    }
}
```

The `ref` attribute gives us a reference to a `TemporaryFile`. This is the default way the `multipartFormData` parser
handles file uploads.

<small>**Note:** As always, we can also use the `anyContent` body parser and retrieve it as
`request.body.asMultipartFormData`.</small>

At last, we add a `POST` router:

```scala
POST  /          democontrollers.HomeController.upload()
```

<small>**Note:** An empty file will be treated just like no file was uploaded at all. The same applies if the `filename`
header of a `multipart/form-data` file upload part is empty - even when the file itself would not be empty.</small>

### Direct file upload
Another way to send files to the server is to use **AJAX** to upload files asynchronously from a form. In this case, the
request body will not be encoded as `multipart/form-data`, but will just contain the plain file contents.

In this case we can just use a body parser to store the request body content in a file. For this example, let's use the
`temporaryFile` body parser:

```scala
def upload = Action(parse.temporaryFile) { request =>
  request.body.moveTo(Paths.get("/tmp/picture/uploaded"), replace = true)
  Ok("File uploaded")
}
```

### Writing our own body parser
If we want to handle the file upload directly without buffering it in a temporary file, we can just write our own
`BodyParser`. In this case, we will receive chunks of data that we are free to push anywhere we want.

If we want to use `multipart/form-data` encoding, we can still use the default `multipartFormData` parser by providing a
`FilePartHnadler[A]` and using a different Sink to accumulate data. For example, we can use a `FilePartHandler[File]`
rather than a TemporaryFile by specifying an `Accumulator(fileSink)`:

```scala
type FilePartHandler[A] = FileInfo => Accumulator[ByteString, FilePart[A]]

def handleFilePartAsFile: FilePartHandler[File] = {
  case FileInfo(partName, filename, contentType, dispositionType) =>
    val perms       = java.util.EnumSet.of(OWNER_READ, OWNER_WRITE)
    val attr        = PosixFilePermissions.asFileAttribute(perms)
    val path        = JFiles.createTempFile("multipartBody", "tempFile", attr)
    val file        = path.toFile
    val fileSink    = FileIO.toPath(path)
    val accumulator = Accumulator(fileSink)
    accumulator.map {
      case IOResult(count, status) =>
        FilePart(partName, filename, contentType, file, count, dispositionType)
    }(ec)
}

def uploadCustom = Action(parse.multipartFormData(handleFilePartAsFile)) { request =>
  val fileOption = request.body.file("name").map {
    case FilePart(key, filename, contentType, file, fileSize, dispositionType) =>
      file.toPath
  }

  Ok(s"File uploaded: $fileOption")
}
```

### Cleaning up temporary files
Uploading files uses a `TemporaryFile` **API** which relies on storing files in a temporary filesystem, accessible
through the `ref` attribute. All `TemporaryFile` references come from a `TemporaryFileCreator` trait, and the
implementation can be swapped out as necessary, and there's now an `atomicMoveWithFallback` method that uses
`StandardCopyOption.ATOMIC_MOVE` if available.

Uploading files is an inherently dangerous operation, because unbounded file upload can cause the filesystem to fill up
-- as such, the idea behind `TemporaryFile` is that it's only in scope at completion and should be moved out of the
temporary file system as soon as possible. Any temporary files that are not moved are deleted.

However, under [**certain conditions**](https://github.com/playframework/playframework/issues/5545), garbage
collection does not occur in a timely fashion. As such, there's also a `play.api.libs.Files.TemporaryFileReaper` that
can be enabled to delete temporary files on a scheduled basis using the **Akka** scheduler, distinct from the garbage
collection method.

The reaper is disabled by default, and is enabled through configuration in `application.conf`:

```scala
play.temporaryFile {
  reaper {
    enabled = true
    initialDelay = "5 minutes"
    interval = "30 seconds"
    olderThan = "30 minutes"
  }
}
```

The above configuration will delete files that are more than 30 minutes old, using the *"olderThan"* property. It will
start the reaper five minutes after the application starts, and will check the filesystem every 30 seconds thereafter.
The reaper is not aware of any existing file uploads, so protracted file uploads may run into the reaper if the system
is not carefully configured.

## Protecting against Cross Site Request Forgery
Cross Site Request Forgery (**CSRF**) is a security exploit where an attacker tricks a victim's browser into making a
request using the victim's session. Since the session token is sent with every request, if an attacker can coerce the
victim's browser to make a request on their behalf, the attacker can make requests on the user's behalf.

There is no simple answer to what requests are safe and what are vulnerable to **CSRF** attacks; the reason for this is
that there is no clear specification as to what is allowable from plugins and future extensions to specifications.
Historically, browser plugins and extensions have relaxed the rules that frameworks previously thought could be trusted,
introducing **CSRF** vulnerabilities to many applications, and the onus has been on the frameworks to fix them. For this
reason, **Play** takes a conservative approach in its defaults, but allows us to configure exactly when a check is done.
By default, **Play** will require a **CSRF** check when all of the following are true:

- The request method is not `GET`, `HEAD` or `OPTIONS`
- The request has one or more `Cookie` or `Authorization` headers
- The **CORS** filter is not configured to trust the request's origin

<small>**Note:** If we use browser-based authentication other than using cookies or **HTTP** authentication, such as
**NTLM** or client certificate-based authentication, then we **must** set `play.filters.csrf.header.protectHeaders =
null`, which will protect all requests, or include the headers used in authentication in `protectHeaders`.</small>

### Play's CSRF protection
**Play** supports multiple methods for verifying that a request is not a **CSRF** request. The primary mechanism is a
**CSRF** token. This token gets placed either in the query string or body of every form submitted, and also gets placed
in the user's session. **Play** then verifies that both tokens are present and match.

To allow simple protection for non-browser requests, **Play** checks requests with `Cookie` or `Authorization` headers
by default. We can configure `play.filters.csrf.header.protectHeaders` to define headers that must be present to perform
the **CSRF** check. If we are making requests with **AJAX**, we can place the **CSRF** token in the **HTML** page, and
then add it to the request using the `Csrf-Token` header.

Alternatively, we can set `play.filters.csrf.header.bypassHeaders` to match common headers. A common configuration would
be:

- If a `X-Requested-With` header is present, **Play** will consider the request safe. `X-Requested-With` is added to
  requests by many popular **JavaScript** libraries, such as **jQuery**.

- If a `Csrf-Token` header with value `nocheck` is present, or with a valid **CSRF** token, **Play** will consider the
  request safe.

This configuration would look like:

```scala
play.filters.csrf.header.bypassHeaders {
  X-Requested-With = "*"
  Csrf-Token = "nocheck"
}
```

Caution should be taken when using this configuration option, as historically browser plugins have undermined this type
of **CSRF** defence.


#### Trusting CORS requests
By default, if we have a **CORS** filter before our **CSRF** filter, the **CSRF** filter will let through **CORS**
requests from trusted origins. To disable this check, we must set the config option
`play.filters.csrf.bypassCorsTrustedOrigins = false`.

#### Applying a global CSRF filter
<small>**Note:** As of **Play 2.6.x**, the **CSRF** filter is included in **Play's** list of default filters that are
applied automatically to projects.</small>

**Play** provides a global **CSRF** filter that can be applied to all requests. This is the simplest way to add **CSRF**
protection to an application. To add the filter manually, we add it to `application.conf`:

```scala
play.filters.enabled += "play.filters.csrf.CSRFFilter"
```

It is also possible to disable the **CSRF** filter for a specific route in the routes file. To do this, we add the
`nocsrf` modifier tag before our route:

```scala
+ nocsrf
POST  /api/new              controllers.Api.newThing
```

#### Using an implicit request
All **CSRF** functionality assumes that an implicit `RequestHeader` (or a `Request`, which extends `RequestHeader`) is
available in implicit scope, and will not compile without one available. Examples will be shown below.

#### Defining an implicit Request in Actions
For all the actions that need to access the **CSRF** token, the request must be exposed implicitly with `implicit
request =>` as follows:

```scala
// this actions needs to access CSRF token
def someMethod = Action { implicit request =>
  // access the token as you need
  Ok
}
```

That is because the helper methods like `CSRF.getToken` receive the request as an implicit parameter to retrieve the
**CSRF** token, for example:

```scala
def someAction = Action { implicit request =>
  accessToken // request is passed implicitly to accessToken
  Ok("success")
}

def accessToken(implicit request: Request[_]) = {
  val token = CSRF.getToken // request is passed implicitly to CSRF.getToken
}
```

#### Passing an implicit Request between methods
If we have broken up our code into methods that **CSRF** functionality is used in, then we can pass through the implicit
request from the action:

```scala
def action = Action { implicit request =>
  anotherMethod("Some para value")
  Ok
}

def anotherMethod(p: String)(implicit request: Request[_]) = {
  // do something that needs access to the request
}
```

#### Defining an implicit Request in Templates
Our **HTML** template should have an implicit `RequestHeader` parameter to our template, if it doesn't have one already,
because the `CSRF.formField` helper requires one to be passed in (discussed more below):

```scala
@(...)(implicit request: RequestHeader)
```

Since we will typically use **CSRF** in conjunction with form helpers that require a `MessagesProvider` instance, we may
want to use `MessagesAbstractController` or another controller which provides a `MessagesRequestHeader`:

```scala
@(...)(implicit request: MessagesRequestHeader)
```

Or, if we are using a controller with `I18nSupport` we can pass in the messages as a separate implicit parameter:

```scala
@(...)(implicit request: RequestHeader, messages: Messages)
```

#### Getting the current token
The current **CSRF** token can be accessed using the `CSRF.getToken` method. It takes an implicit `RequestHeader`, so we
must ensure that one is in scope.

```scala
val token: Option[CSRF.Token] = CSRF.getToken
```

<small>**Note:** If the **CSRF** filter is installed, **Play** will try to avoid generating the token as long as the
cookie being used is **HttpOnly** (meaning it cannot be accessed from **JavaScript**). When sending a response with a
strict body, **Play** skips adding the token to the response unless `CSRF.getToken` has already been called. This
results in a significant performance improvement for responses that don't need a **CSRF** token. If the cookie is not
configured to be **HttpOnly**, **Play** will assume we wish to access it from **JavaScript** and generate it
regardless.</small>

If we are not using the **CSRF** filter, we also should inject the `CSRFAddToken` and `CSRFCheck` action wrappers to
force adding a token or a **CSRF** check on a specific action. Otherwise the token will not be available.

```scala
import play.api.mvc._
import play.api.mvc.Results._
import play.filters.csrf._
import play.filters.csrf.CSRF.Token

class CSRFController(components: ControllerComponents, addToken: CSRFAddToken, checkToken: CSRFCheck)
    extends AbstractController(components) {
  def getToken =
    addToken(Action { implicit request =>
      val Token(name, value) = CSRF.getToken.get
      Ok(s"$name=$value")
    })
}
```

To help in adding **CSRF** token to forms, **Play** provides some template helpers. The first one adds it to the query
string of the action **URL**:

```scala
@import helper._

@form(CSRF(routes.ItemsController.save())) {
  ...
}
```

This might render a form that looks like this:

```html
<form method="POST" action="/items?csrfToken=1234567890abcdef">
  ...
</form>
```

If it is undesirable to have the token in the query string, **Play** also provides a helper for adding the **CSRF**
token as hidden field in the form:

```scala
@form(routes.ItemsController.save()) {
  @CSRF.formField
  ...
}
```

This might render a form that looks like this:

```scala
<form method="POST" action="/items">
  <input type="hidden" name="csrfToken" value="1234567890abcdef"/>
  ...
</form>
```

#### Adding a CSRF token to the session
To ensure that a **CSRF** token is available to be rendered in forms, and sent back to the client, the global filter
will generate a new token for all `GET` requests that accept **HTML**, if a token isn't already available in the
incoming request.


### Applying CSRF filtering on a per action basis
Sometimes global **CSRF** filtering may not be appropriate, for example in situations where an application might want to
allow some cross origin form posts. Some non session based standards, such as **OpenID 2.0**, require the use of cross
site form posting, or use form submission in server to serve **RPC** communications.

In these cases, **Play** provides two actions that can be composed with our application's actions.

The first action is the `CSRFCheck` action, and it performs the check. It should be added to all actions that accept
session authenticated `POST` form submissions:

```scala
import play.api.mvc._
import play.filters.csrf._

def save = checkToken {
  Action { implicit req =>
    // handle body
    Ok
  }
}
```

The second action is the `CSRFAddToken` action, it generates a **CSRF** token if not already present on the incoming
request. It should be added to all actions that render forms:

```scala
import play.api.mvc._
import play.filters.csrf._

def form = addToken {
  Action { implicit req =>
    Ok(views.html.itemsForm)
  }
}
```

A more convenient way to apply these actions is to use them in combination with **Play's** action composition:

```scala
import play.api.mvc._
import play.filters.csrf._

class PostAction @Inject() (parser: BodyParsers.Default) extends ActionBuilderImpl(parser) {
  override def invokeBlock[A](request: Request[A], block: (Request[A]) => Future[Result]) = {
    // authentication code here
    block(request)
  }
  override def composeAction[A](action: Action[A]) = checkToken(action)
}

class GetAction @Inject() (parser: BodyParsers.Default) extends ActionBuilderImpl(parser) {
  override def invokeBlock[A](request: Request[A], block: (Request[A]) => Future[Result]) = {
    // authentication code here
    block(request)
  }
  override def composeAction[A](action: Action[A]) = addToken(action)
}
```

Then we can minimize the boiler plate code necessary to write actions:

```scala
def save = postAction {
  // handle body
  Ok
}

def form = getAction { implicit req =>
  Ok(views.html.itemsForm)
}
```

### CSRF configuration options
The full range of **CSRF** configuration options can be found in the filters
[**reference.conf**](https://www.playframework.com/documentation/2.8.x/resources/confs/filters-helpers/reference.conf).
Some examples include:

- `play.filters.csrf.token.name` - The name of the token to use both in the session and in the request body/query
  string. Defaults to `csrfToken`.

- `play.filters.csrf.cookie.name` - If configured, **Play** will store the **CSRF** token in a cookie with the given
  name, instead of in the session.

- `play.filters.csrf.cookie.secure` - If `play.filters.csrf.cookie.name` is set, whether the **CSRF** cookie should have
  the secure flag set. Defaults to the same value as `play.http.session.secure`.

- `play.filters.csrf.body.bufferSize` - In order to read tokens out of the body, **Play** must first buffer the body and
  potentially parse it. This sets the maximum buffer size that will be used to buffer the body. Defaults to 100k.

- `play.filters.csrf.token.sign` - Whether **Play** should use signed **CSRF** tokens. Signed **CSRF** tokens ensure
  that the token value is randomised per request, thus defeating **BREACH** style attacks.

### Using CSRF with compile time dependency injection
We can use all the above features if our application is using compile time dependency injection. The wiring is helped by
the trait `CSRFComponents` that we can mix in our application components cake.

### Testing CSRF
When rendering, we may need to add the **CSRF** token to a template. We can do this with `import
play.api.test.CSRFTokenHelper._`, which enriches `play.api.test.FakeRequest` with the `withCSRFToken` method:

```scala
import play.api.test.Helpers._
import play.api.test.CSRFTokenHelper._
import play.api.test.FakeRequest
import play.api.test.WithApplication

class UserControllerSpec extends Specification {
  "UserController GET" should {
    "render the index page from the application" in new WithApplication() {
      val controller = app.injector.instanceOf[UserController]
      val request    = FakeRequest().withCSRFToken
      val result     = controller.userGet().apply(request)

      status(result) must beEqualTo(OK)
      contentType(result) must beSome("text/html")
    }
  }
}
```

## Internationalization with Messages
### Specifying languages supported by our application
We specify languages for our application using language tags, specially formatted strings that identify a specific
language. Language tags can specify simple languages, such as "en" for English, a specific regional dialect of a
language (such as "en-AU" for English as used in Australia), a language and a script (such as "az-Latn" for Azerbaijani
written in Latin script), or a combination of several of these (such as "zh-cmn-Hans-CN" for Chinese, Mandarin,
Simplified script, as used in China).

To start, we need to specify the languages supported by our application in the `conf/application.conf` file:

```scala
play.i18n.langs = [ "en", "en-US", "fr" ]
```

These language tags will be used to create `play.api.i18n.Lang` instances. To access the languages supported by our
application, we can inject a `play.api.i18n.Langs` component into our class:

```scala
import javax.inject.Inject

import play.api.i18n.Lang
import play.api.i18n.Langs
import play.api.mvc.BaseController
import play.api.mvc.ControllerComponents

class ScalaI18nService @Inject() (langs: Langs) {
  val availableLangs: Seq[Lang] = langs.availables
}
```

An individual `play.api.i18n.Lang` can be converted to a `java.util.Locale` object by using `lang.toLocale`:

```scala
val locale: java.util.Locale = lang.toLocale
```

### Externalizing messages
We can externalize messages in the `conf/messages.xxx` files.

The default `conf/messages` file matches all languages. Additionally we can specify language-specific message files such
as `conf/messages.fr` or `conf/messages.en-US`.

Messages are available through the `MessagesApi` instance, which can be added via injection. We can then retrieve
messages using the `play.api.i18n.MessagesApi` object:

```scala
import play.api.i18n.MessagesApi

class MyService @Inject() (langs: Langs, messagesApi: MessagesApi) {
  val lang: Lang = langs.availables.head

  val title: String = messagesApi("home.title")(lang)
}
```

We can also make the language implicit rather than declare it:

```scala
class MyOtherService @Inject() (langs: Langs, messagesApi: MessagesApi) {
  implicit val lang: Lang = langs.availables.head

  lazy val title: String = messagesApi("home.title")
}
```

**Play** provides predefined messages for forms validation. We can overwrite these messages either with the default
message file or any language-specific message file. We can see below which messages can be overwritten:

```ini
#
# Copyright (C) Lightbend Inc. <https://www.lightbend.com>
#

# Default messages

# --- Constraints
constraint.required=Required
constraint.min=Minimum value: {0}
constraint.max=Maximum value: {0}
constraint.minLength=Minimum length: {0}
constraint.maxLength=Maximum length: {0}
constraint.email=Email
constraint.pattern=Required pattern: {0}

# --- Formats
format.date=Date (''{0}'')
format.numeric=Numeric
format.real=Real
format.uuid=UUID

# --- Patterns for Formats
formats.date=yyyy-MM-dd

# --- Errors
error.invalid=Invalid value
error.invalid.java.util.Date=Invalid date value
error.required=This field is required
error.number=Numeric value expected
error.real=Real number value expected
error.real.precision=Real number value with no more than {0} digit(s) including {1} decimal(s) expected
error.min=Must be greater or equal to {0}
error.min.strict=Must be strictly greater than {0}
error.max=Must be less or equal to {0}
error.max.strict=Must be strictly less than {0}
error.minLength=Minimum length is {0}
error.maxLength=Maximum length is {0}
error.email=Valid email required
error.pattern=Must satisfy {0}
error.date=Valid date required
error.uuid=Valid UUID required

error.expected.date=Date value expected
error.expected.date.isoformat=Iso date value expected
error.expected.time=Time value expected
error.expected.jsarray=Array value expected
error.expected.jsboolean=Boolean value expected
error.expected.jsnumber=Number value expected
error.expected.jsobject=Object value expected
error.expected.jsstring=String value expected
error.expected.jsnumberorjsstring=String or number expected
error.expected.keypathnode=Node value expected
error.expected.uuid=UUID value expected
error.expected.validenumvalue=Valid enumeration value expected
error.expected.enumstring=String value expected

error.path.empty=Empty path
error.path.missing=Missing path
error.path.result.multiple=Multiple results for the given path
```

### Using Messages and MessagesProvider
Because it's common to want to use messages without having to provide an argument, we can wrap a given `Lang` together
with the `MessagesApi` to create a `play.api.i18n.Messages` instance. The `play.api.i18n.MessagesImpl` case class
implements the `Messages` trait if we want to create one directly:

```scala
val messages: Messages = MessagesImpl(lang, messagesApi)
val title: String      = messages("home.title")
```

We can also use **Singleton** object methods with an implicit `play.api.i18n.MessagesProvider`:

```scala
implicit val messagesProvider: MessagesProvider = {
  MessagesImpl(lang, messagesApi)
}
// uses implicit messages
val title2 = Messages("home.title")
```

A `play.api.i18n.MessagesProvider` is a trait that can provide a `Messages` object on demand. An instance of `Messages`
extends `MessagesProvider` and returns itself.

`MessagesProvider` is most useful when extended by something that is not a `Messages`:

```scala
implicit val customMessagesProvider: MessagesProvider = new MessagesProvider {
  // resolve messages at runtime
  override def messages: Messages = { ... }
}
// uses implicit messages
val title3: String = Messages("home.title")
```

### Using Messages with Controllers
We can add `Messages` support to our request by extending `MessagesAbstractController` or `MessagesBaseController`:

```scala
import javax.inject.Inject
import play.api.i18n._

class MyMessagesController @Inject() (mcc: MessagesControllerComponents) extends MessagesAbstractController(mcc) {
  def index = Action { implicit request: MessagesRequest[AnyContent] =>
    val messages: Messages = request.messages
    val message: String    = messages("info.error")
    Ok(message)
  }

  def messages2 = Action { implicit request: MessagesRequest[AnyContent] =>
    val lang: Lang      = request.messages.lang
    val message: String = messagesApi("info.error")(lang)
    Ok(message)
  }

  def messages4 = Action { implicit request: MessagesRequest[AnyContent] =>
    // MessagesRequest is an implicit MessagesProvider
    Ok(views.html.formpage())
  }
}
```

Or by adding the `play.api.i18n.I18nSupport` trait to our controller and ensuring an instance of `MessagesApi` is in
scope, which will use implicits to convert a request.

```scala
import javax.inject.Inject
import play.api.i18n._

class MySupportController @Inject() (val controllerComponents: ControllerComponents)
    extends BaseController
    with I18nSupport {
  def index = Action { implicit request =>
    // type enrichment through I18nSupport
    val messages: Messages = request.messages
    val message: String    = messages("info.error")
    Ok(message)
  }

  def messages2 = Action { implicit request =>
    // type enrichment through I18nSupport
    val lang: Lang      = request.lang
    val message: String = messagesApi("info.error")(lang)
    Ok(message)
  }

  def messages3 = Action { request =>
    // direct access with no implicits required
    val messages: Messages = messagesApi.preferred(request)
    val lang               = messages.lang
    val message: String    = messages("info.error")
    Ok(message)
  }

  def messages4 = Action { implicit request =>
    // takes implicit Messages, converted using request2messages
    // template defined with @()(implicit messages: Messages)
    Ok(views.html.formpage())
  }
}
```

All the form helpers in **Twirl** templates take `MessagesProvider`, and it is assumed that a `MessagesProvider` is
passed into the template as an implicit parameter when processing a form.

```scala
@(form: Form[Foo])(implicit messages: MessagesProvider)

@helper.inputText(field = form("name")) @* <- takes MessagesProvider *@
```

### Retrieving supported language from an HTTP request
We can retrieve the languages supported by a specific **HTTP** request:

```scala
def index = Action { request =>
  Ok("Languages: " + request.acceptLanguages.map(_.code).mkString(", "))
}
```

### Request types
The `I18nSupport` trait adds the following methods to a `Request`:

- `request.messages` returns an instance of `Messages`, using an implicit `MessagesApi`
- `request.lang` returns the preferred `Lang`, using an implicit `MessagesApi`

The preferred language is extracted from the `Accept-Language` header (and optionally the language cookie) and matching
one of the `MessagesApi` supported languages using `messagesApi.preferred`.

### Language cookie support
The `I18nSupport` also adds two convenient methods to `Result`:

- `result.withLang(lang: Lang)` is used to set the language using **Play's** language cookie.
- `result.withoutLang` is used to clear the language cookie.

For example:

```scala
def homePageInFrench = Action {
  Redirect("/user/home").withLang(Lang("fr"))
}

def homePageWithDefaultLang = Action {
  Redirect("/user/home").withoutLang
}
```

The `withLang` method sets the cookie named `PLAY_LANG` for future requests, while `withoutLang` discards the cookie,
and **Play** will choose the language based on the client's `Accept-Language` header.

The cookie name can be changed by changing the configuration parameter: `play.i18n.langCookieName`.

### Implicit Lang Conversion
The `LangImplicits` trait can be declared on a controller to implicitly convert a request to a `Messages` given an
implicit `Lang` instance.

```scala
import play.api.i18n.LangImplicits

class MyClass @Inject() (val messagesApi: MessagesApi) extends LangImplicits {
  def convertToMessage: Unit = {
    implicit val lang      = Lang("en")
    val messages: Messages = lang2Messages // implicit conversion
  }
}
```

### Messages format
Messages are formatted using the `java.text.MessageFormat` library. For example, assuming we have a message defined
like:

```ini
files.summary=The disk {1} contains {0} file(s).
```

We can then specify parameters as:

```scala
Messages("files.summary", d.files.length, d.name)
```

### Notes on apostrophes
Since `Messages` uses `java.text.MessageFormat`, we must be aware that single quotes are used as a meta-character for
escaping parameter substitutions.

For example, if we have the following messages defined:

```ini
info.error=You aren''t logged in!
example.formatting=When using MessageFormat, '''{0}''' is replaced with the first parameter.
```

we should expect the following results:

```scala
messagesApi("info.error") == "You aren't logged in!"
messagesApi("example.formatting") == "When using MessageFormat, '{0}' is replaced with the first parameter."
```

## Session and Flash scopes
### How it is different in Play
If we have to keep data across multiple **HTTP** requests, we can save them in the **Session** or the **Flash** scope.
Data stored in the **Session** is available during the whole user session, and data stored in the flash scope is only
available in the next request.

### Working with Cookies
It's important to understand that **Session** and **Flash** data is not stored in the server but is added to each
subsequent **HTTP** request, using **HTTP** cookies.

Because **Session** and **Flash** are implemented using cookies, there are some important implications:

- The data size is very limited (up to 4 KB).
- We can only store string values, although we can serialize **JSON** to the cookie.
- Information in a cookie is visible to the browser, and so can expose sensitive data.
- Cookie information is immutable to the original request, and only available to subsequent requests.

The last point can be a source of confusion. When we modify the cookie, we are providing information to the response,
and **Play** must parse it again to see the updated value. If we would like to ensure the session information is current
then we should always pair modification of a session with a redirect.

### Session configuration
The default name for the cookie is `PLAY_SESSION`. This can be changed by configuring the key
`play.http.session.cookieName` in `application.conf`.

If the name of the cookie is changed, the earlier cookie can be discarded.

#### Session timeout / expiration
By default, there is no technical timeout for the **Session**. It expires when the user closes the web browser. If we
need a functional timeout for a specific application, we set the maximum age of the session cookie by configuring the
key `play.http.session.maxAge` in `application.conf`, and this will also set `play.http.session.jwt.expiresAfter` to the
same value. The `maxAge` property will remove the cookie from the browser, and the **JWT** `exp` claim will be set in
the cookie, and will make it invalid after the given duration.

### Storing data in the session
As the session is just a cookie, it is also just an **HTTP** header. We can manipulate the session data the same way we
manipulate other result properties:

```scala
Redirect("/home").withSession("connected" -> "user@gmail.com")
```

This will replace the whole session. If we need to add an element to an existing session, we should just add an element
to the incoming session, and specify that as new session:

```scala
Redirect("/home").withSession(request.session + ("saidHello" -> "yes"))
```

We can remove any value from the incoming session in the same way:

```scala
Redirect("/home").withSession(request.session - "theme")
```

### Reading a session value
We can retrieve the incoming session from the **HTTP** request:

```scala
def index = Action { request =>
  request.session
    .get("connected")
    .map { user =>
      Ok("Hello " + user)
    }
    .getOrElse {
      Unauthorized("Oops, you are not connected")
    }
}
```

### Discarding the whole session
There is a special operation that discards the whole session:

```scala
Redirect("/home").withNewSession
```

### Flash scope
The **Flash** scope works exactly like the **Session**, but with one difference: data is kept for only one request.

<small>**Important:** The **Flash** scope should only be used to transport success/error messages on simple non-**Ajax**
applications. As the data is just kept for the next request and because there are no guarantees to ensure the request
order in a complex Web application, the **Flash** scope is subject to race conditions.</small>

Here are a few examples using the **Flash** scope:

```scala
def index = Action { implicit request =>
  Ok {
    request.flash.get("success").getOrElse("Welcome!")
  }
}

def save = Action {
  Redirect("/home").flashing("success" -> "The item has been created")
}
```

To retrieve the **Flash** scope value in our view, we add an implicit **Flash** parameter:

```scala
@()(implicit flash: Flash)
...
@flash.get("success").getOrElse("Welcome!")
...
```

And in our `Action`, we specify an `implicit request =>` as shown below:

```scala
def index = Action { implicit request =>
  Ok(views.html.index())
}
```

An implicit **Flash** will be provided to the view based on the implicit request.

If the error *'could not find implicit value for parameter flash: play.api.mvc.Flash'* is raised then this is because
our `Action` didn't have an implicit request in scope.

## Body parsers
### What is a body parser?
An **HTTP** request is a header followed by a body. The header is typically small - it can be safely buffered in memory,
hence in **Play** it is modelled using the `RequestHeader` class. The body however can be potentially very long, and so
is not buffered in memory, but rather modelled as a stream. However, many request body payloads are small and can be
modelled in memory, and so to map the body stream to an object in memory, **Play** provides a `BodyParser` abstraction.

Since **Play** is an asynchronous framework, the traditional `InputStream` can't be used to read the request body -
input streams are blocking, when we invoke `read`, the thread invoking it must wait for data to be available. Instead,
**Play** uses an asynchronous streaming library called [**Akka
Streams**](https://doc.akka.io/docs/akka/2.6/stream/index.html?language=scala). **Akka Streams** is an implementation of
[**Reactive Streams**](http://www.reactive-streams.org/), a **SPI** that allows many asynchronous streaming **APIs** to
seamlessly work together, so though traditional `InputStream`-based technologies are not suitable for use with **Play**,
**Akka Streams** and the entire ecosystem of asynchronous libraries around **Reactive Streams** will provide us with
everything we need.

### More about Actions
Previously we said that an `Action` was a `Request => Result` function. This is not entirely true. Let's have a more
precise look at the `Action` trait:

```scala
trait Action[A] extends (Request[A] => Result) {
  def parser: BodyParser[A]
}
```

First we see that there is a generic type `A`, and then that an action must define a `BodyParser[A]`. With `Request[A]`
being defined as:

```scala
trait Request[+A] extends RequestHeader {
  def body: A
}
```

The `A` type is the type of the request body. We can use any **Scala** type as the request body, for example `String`,
`NodeSeq`, `Array[Byte]`, `JsonValue`, or `java.io.File`, as long as we have a body parser able to process it.

To summarize, an `Action[A]` uses a `BodyParser[A]` to retrieve a value of type `A` from the **HTTP** request, and to
build a `Request[A]` object that is passed to the action code.

### Using the built in body parsers
Most typical web apps will not need to use custom body parsers, they can simply work with **Play's** built-in body
parsers. These include parsers for **JSON**, **XML**, forms, as well as handling plain text bodies as `Strings` and byte
bodies as `ByteString`.

#### The default body parser
The default body parser that's used if we do not explicitly select a body parser will look at the incoming
`Content-Type` header, and parse the body accordingly. So for example, a `Content-Type` of type `application/json` will
be parsed as a `JsValue`, while a `Content-Type` of `application/x-www-form-urlencoded` will be parsed as a `Map[String,
Seq[String]]`.

The default body parser produces a body of type `AnyContent`. The various types supported by `AnyContent` are accessible
via `as` methods, such as `asJson`, which returns an `Option` of the body type:

```scala
def save = Action { request: Request[AnyContent] =>
  val body: AnyContent          = request.body
  val jsonBody: Option[JsValue] = body.asJson

  // Expecting json body
  jsonBody
    .map { json =>
      Ok("Got: " + (json \ "name").as[String])
    }
    .getOrElse {
      BadRequest("Expecting application/json request body")
    }
}
```

The following is a mapping of types supported by the default body parser:

- **text/plain:** `String`, accessible via `asText`.
- **application/json:** `JsValue`, accessible via `asJson`.
- **application/xml**, **text/xml** or **application/XXX+xml:** `scala.xml.NodeSeq`, accessible via `asXml`.
- **application/x-www-form-urlencoded:** `Map[String, Seq[String]]`, accessible via `asFormUrlEncoded`.
- **multipart/form-data:** `MultipartFormData`, accessible via `asMultipartFormData`.
- Any other content type: `RawBuffer`, accessible via `asRaw`.

The default body parser tries to determine if the request has a body before it tries to parse. According to the **HTTP**
spec, the presence of either the `Content-Length` or `Transfer-Encoding` header signals the presence of a body, so the
parser will only parse if one of those headers is present, or on `FakeRequest` when a non-empty body has explicitly been
set.

If we would like to try to parse a body in all cases, we can use the `anyContent` body parser, described below.

### Choosing an explicit body parser
If we want to explicitly select a body parser, this can be done by passing a body parser to the `Action` `apply` or
`async` method.

**Play** provides a number of body parsers out of the box. This is made available through the `PlayBodyParsers` trait,
which can be injected into our controller.

So for example, to define an action expecting a json body (as in the previous example):

```scala
def save = Action(parse.json) { request: Request[JsValue] =>
  Ok("Got: " + (request.body \ "name").as[String])
}
```

Note this time that the type of the body is `JsValue`, which makes it easier to work with the body since it's no longer
an `Option`. The reason why it's not an `Option` is because the json body parser will validate that the request has a
`Content-Type` of `application/json`, and send back a `415 Unsupported Media Type` response if the request doesn't meet
that expectation. Hence we don't need to check again in our action code.

This of course means that clients have to be well behaved, sending the correct `Content-Type` headers with their
requests. If we want to be a little more relaxed, we can instead use `tolerantJson`, which will ignore the
`Content-Type` and try to parse the body as json regardless:

```scala
def save = Action(parse.tolerantJson) { request: Request[JsValue] =>
  Ok("Got: " + (request.body \ "name").as[String])
}
```

Here is another example, which will store the request body in a file:

```scala
def save = Action(parse.file(to = new File("/tmp/upload"))) { request: Request[File] =>
  Ok("Saved the request content to " + request.body)
}
```

#### Combining body parsers
In the previous example, all request bodies are stored in the same file, which is a bit problematic. Let's write another
custom body parser that extracts the user name from the request **Session**, to give a unique file for each user:

```scala
val storeInUserFile = parse.using { request =>
  request.session
    .get("username")
    .map { user =>
      parse.file(to = new File("/tmp/" + user + ".upload"))
    }
    .getOrElse {
      sys.error("You don't have the right to upload here")
    }
}

def save = Action(storeInUserFile) { request =>
  Ok("Saved the request content to " + request.body)
}
```

<small>**Note:** Here we are not really writing our own `BodyParser`, but just combining existing ones. This is often
enough and should cover most use cases.</small>

#### Max content length
Text-based body parsers (such as `text`, `json`, `xml`, or `formUrlEncoded`) use a max content length because they have
to load all the content into memory. By default, the maximum content length that they will parse is **100KB**. It can be
overridden by specifying the `play.http.parser.maxMemoryBuffer` property in `application.conf`:

```ini
play.http.parser.maxMemoryBuffer=128K
```

For parsers that buffer content on disk, such as the raw parser or `multipart/form-data`, the maximum content length is
specified using the `play.http.parser.maxDiskBuffer` property. It defaults to **10MB**. The `multipart/form-data` parser
also enforces the text max length property for the aggregate of the data fields.

We can also override the default maximum length for a given action:

```scala
// Accept only 10KB of data.
def save = Action(parse.text(maxLength = 1024 * 10)) { request: Request[String] =>
  Ok("Got: " + text)
}
```

We can also wrap any body parser with `maxLength`:

```scala
// Accept only 10KB of data.
def save = Action(parse.maxLength(1024 * 10, storeInUserFile)) { request =>
  Ok("Saved the request content to " + request.body)
}
```

### Writing a custom body parser
A custom body parser can be made by implementing the `BodyParser` trait. This trait is simply a function:

```scala
trait BodyParser[+A] extends (RequestHeader => Accumulator[ByteString, Either[Result, A]])
```

The signature of this function may be a bit daunting at first, so let's break it down.

The function takes a `RequestHeader`. This can be used to check information about the request - most commonly, it is
used to get the `Content-Type`, so that the body can be correctly parsed.

The return type of the function is an `Accumulator`. An accumulator is a thin layer around an [**Akka Streams
Sink**](https://doc.akka.io/api/akka/2.5/index.html#akka.stream.scaladsl.Sink). An accumulator asynchronously
accumulates streams of elements into a result. It can be run by passing in an [**Akka Streams
Source**](https://doc.akka.io/api/akka/2.5/index.html#akka.stream.scaladsl.Source). This will return a `Future` that
will be redeemed when the accumulator is complete. It is essentially the same thing as a `Sink[E, Future[A]]`. In fact,
it is nothing more than a wrapper around this type, but the big difference is that `Accumulator` provides convenient
methods such as `map`, `mapFuture`, `recover`, etc. for working with the result as if it were a promise, where `Sink`
requires all such operations to be wrapped in a `mapMaterializedValue` call.

The accumulator that the `apply` method returns consumes elements of type `ByteString` - these are essentially arrays of
bytes, but differ from `byte[]` in that `ByteString` is immutable, and many operations such as slicing and appending
happen in constant time.

The return type of the accumulator is `Either[Result, A]` - it will either return a `Result`, or it will return a body
of type `A`. A result is generally returned in the case of an error, for example, if the body failed to be parsed, if
the `Content-Type` didn't match the type that the body parser accepts, or if an in-memory buffer was exceeded. When the
body parser returns a result, this will short circuit the processing of the action - the body parser's result will be
returned immediately, and the action will never be invoked.

#### Directing the body elsewhere
One common use case for writing a body parser is for when we actually don't want to parse the body, rather, we want to
stream it elsewhere. To do this, we may define a custom body parser:

```scala
import javax.inject._
import play.api.mvc._
import play.api.libs.streams._
import play.api.libs.ws._
import scala.concurrent.ExecutionContext
import akka.util.ByteString

class MyController @Inject() (ws: WSClient, val controllerComponents: ControllerComponents)(
    implicit ec: ExecutionContext
) extends BaseController {
  def forward(request: WSRequest): BodyParser[WSResponse] = BodyParser { req =>
    Accumulator.source[ByteString].mapFuture { source =>
      request
        .withBody(source)
        .execute("POST")
        .map(Right.apply)
    }
  }

  def myAction = Action(forward(ws.url("https://example.com"))) { req =>
    Ok("Uploaded")
  }
}
```

#### Custom parsing using Akka Streams
In rare circumstances, it may be necessary to write a custom parser using **Akka Streams**. In most cases it will
suffice to buffer the body in a `ByteString` first. This will typically offer a far simpler way of parsing since we can
use imperative methods and random access on the body.

However, when that's not feasible, for example when the body we need to parse is too long to fit in memory, then we may
need to write a custom body parser.

The following shows a **CSV** parser:

```scala
import play.api.mvc.BodyParser
import play.api.libs.streams._
import akka.util.ByteString
import akka.stream.scaladsl._

val Action = inject[DefaultActionBuilder]

val csv: BodyParser[Seq[Seq[String]]] = BodyParser { req =>
  // A flow that splits the stream into CSV lines
  val sink: Sink[ByteString, Future[Seq[Seq[String]]]] = Flow[ByteString]
    // We split by the new line character, allowing a maximum of 1000 characters per line
    .via(Framing.delimiter(ByteString("\n"), 1000, allowTruncation = true))
    // Turn each line to a String and split it by commas
    .map(_.utf8String.trim.split(",").toSeq)
    // Now we fold it into a list
    .toMat(Sink.fold(Seq.empty[Seq[String]])(_ :+ _))(Keep.right)

  // Convert the body to a Right either
  Accumulator(sink).map(Right.apply)
}
```

## Accessing an SQL database
<small>**NOTE:** **JDBC** is a blocking operation that will cause threads to wait. We can negatively impact the
performance of our **Play** application by running **JDBC** queries directly in our controller!</small>

### Configuring JDBC connection pools
**Play** provides a plugin for managing **JDBC** connection pools. We can configure as many databases as we need.

To enable the database plugin we add the build dependencies:

```scala
libraryDependencies ++= Seq(
  jdbc
)
```

### Configuring the JDBC Driver dependency
**Play** does not provide any database drivers. Consequently, to deploy in production we will have to add our database
driver as an application dependency.

For example, if we use **MySQL5**, we need to add a dependency for the connector:

```scala
libraryDependencies ++= Seq(
  "mysql" % "mysql-connector-java" % "5.1.41"
)
```

### Databases configuration
Then we must configure a connection pool in the `conf/application.conf` file. By convention, the default **JDBC** data
source must be called `default` and the corresponding configuration properties are `db.default.driver` and
`db.default.url`.

```ini
# Default database configuration
db.default.driver=org.h2.Driver
db.default.url="jdbc:h2:mem:play"
```

If something isn't properly configured, we will be notified directly in our browser:

![DB Config
Error](https://www.playframework.com/documentation/2.8.x/resources/manual/working/commonGuide/database/images/dbError.png)

We can also change the `default` name by setting `play.db.default`, for example:

```ini
play.db.default = "primary"

db.primary.driver=org.h2.Driver
db.primary.url="jdbc:h2:mem:play"
```

#### How to configure several data sources
To configure several data sources:

```ini
# Orders database
db.orders.driver=org.h2.Driver
db.orders.url="jdbc:h2:mem:orders"

# Customers database
db.customers.driver=org.h2.Driver
db.customers.url="jdbc:h2:mem:customers"
```

#### H2 database engine connection properties
In memory database:

```ini
# Default database configuration using H2 database engine in an in-memory mode
db.default.driver=org.h2.Driver
db.default.url="jdbc:h2:mem:play"
```

File-based database:

```ini
# Default database configuration using H2 database engine in a persistent mode
db.default.driver=org.h2.Driver
db.default.url="jdbc:h2:/path/to/db-file"
```

#### SQLite database engine connection properties
```ini
# Default database configuration using SQLite database engine
db.default.driver=org.sqlite.JDBC
db.default.url="jdbc:sqlite:/path/to/db-file"
```

#### PostgreSQL database engine connection properties
```ini
# Default database configuration using PostgreSQL database engine
db.default.driver=org.postgresql.Driver
db.default.url="jdbc:postgresql://database.example.com/playdb"
```

#### MySQL database engine connection properties
```ini
# Default database configuration using MySQL database engine
# Connect to playdb as playdbuser
db.default.driver=com.mysql.jdbc.Driver
db.default.url="jdbc:mysql://localhost/playdb"
db.default.username=playdbuser
db.default.password="a strong password"
```

#### Exposing the datasource through JNDI
Some libraries expect to retrieve the `Datasource` reference from
[**JNDI**](https://docs.oracle.com/javase/tutorial/jndi/overview/index.html). We can expose any **Play**-managed
datasource via **JNDI** by adding this configuration in `conf/application.conf`:

```ini
db.default.driver=org.h2.Driver
db.default.url="jdbc:h2:mem:play"
db.default.jndiName=DefaultDS
```

#### How to configure SQL log statement
Not all connection pools offer (out of the box) a way to log **SQL** statements. Because of that, **Play** uses
[**jdbcdslog-exp**](https://github.com/jdbcdslog/jdbcdslog) to enable consistent **SQL** log statement support for
supported pools. The **SQL** log statement can be configured by database, using `logSql` property:

```ini
# Default database configuration using PostgreSQL database engine
db.default.driver=org.postgresql.Driver
db.default.url="jdbc:postgresql://database.example.com/playdb"
db.default.logSql=true
```

After that, we can configure the **jdbcdslog-exp** log level. Basically, we need to configure our root logger to `INFO`
and then decide what **jdbcdslog-exp** will log (connections, statements and result sets). Here is an example using
`logback.xml` to configure the logs:

```xml
<!--
   Copyright (C) Lightbend Inc. <https://www.lightbend.com>
-->

<configuration>

  <conversionRule conversionWord="coloredLevel" converterClass="play.api.libs.logback.ColoredLevel" />

  <appender name="FILE" class="ch.qos.logback.core.FileAppender">
     <file>${application.home:-.}/logs/application.log</file>
     <encoder>
       <pattern>%date [%level] from %logger in %thread - %message%n%xException</pattern>
     </encoder>
  </appender>

  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%coloredLevel %logger{15} - %message%n%xException{10}</pattern>
    </encoder>
  </appender>

  <appender name="ASYNCFILE" class="ch.qos.logback.classic.AsyncAppender">
    <appender-ref ref="FILE" />
  </appender>

  <appender name="ASYNCSTDOUT" class="ch.qos.logback.classic.AsyncAppender">
    <appender-ref ref="STDOUT" />
  </appender>

  <logger name="play" level="INFO" />

  <logger name="org.jdbcdslog.ConnectionLogger" level="OFF"  /> <!-- Won't log connections -->
  <logger name="org.jdbcdslog.StatementLogger"  level="INFO" /> <!-- Will log all statements -->
  <logger name="org.jdbcdslog.ResultSetLogger"  level="OFF"  /> <!-- Won't log result sets -->

  <root level="WARN">
    <appender-ref ref="ASYNCFILE" />
    <appender-ref ref="ASYNCSTDOUT" />
  </root>

</configuration>
```

<small>**Warning:** This is intended to be used just in development environments and we should not configure it in
production, since there is a performance degradation and it will pollute our logs.</small>

### Accessing the JDBC datasource
**Play** database packages provide access to the default datasource, primarily through the `Database` class.

```scala
import javax.inject.Inject
import play.api.db.Database

import scala.concurrent.Future

class ScalaApplicationDatabase @Inject() (db: Database, databaseExecutionContext: DatabaseExecutionContext) {
  def updateSomething(): Unit = {
    Future {
      db.withConnection { conn =>
        // do whatever you need with the db connection
      }
    }(databaseExecutionContext)
  }
}
```

For a database other than the default:

```scala
import javax.inject.Inject
import play.api.db.Database
import play.db.NamedDatabase

import scala.concurrent.Future

class ScalaNamedDatabase @Inject() (
    @NamedDatabase("orders") ordersDatabase: Database,
    databaseExecutionContext: DatabaseExecutionContext
) {
  def updateSomething(): Unit = {
    Future {
      ordersDatabase.withConnection { conn =>
        // do whatever you need with the db connection
      }
    }(databaseExecutionContext)
  }
}
```

In both cases, when using `withConnection`, the connection will be automatically closed at the end of the block.

### Obtaining a JDBC connection
We can retrieve a **JDBC** connection the same way:

```scala
import javax.inject.Inject
import play.api.db.Database

import scala.concurrent.Future

class ScalaJdbcConnection @Inject() (db: Database, databaseExecutionContext: DatabaseExecutionContext) {
  def updateSomething(): Unit = {
    Future {
      // get jdbc connection
      val connection = db.getConnection()

      // do whatever you need with the db connection

      // remember to close the connection
      connection.close()
    }(databaseExecutionContext)
  }
}
```

It is important to note that resulting **Connections** are not automatically disposed at the end of the request cycle.
In other words, we are responsible for calling their `close()` method somewhere in our code so that they can be
immediately returned to the pool.

### Using a `CustomExecutionContext`
We should always use a custom execution context when using **JDBC**, to ensure that **Play's** rendering thread pool is
completely focused on rendering results and using cores to their full extent. We can use **Play's**
`CustomExecutionContext` class to configure a custom execution context dedicated to serving **JDBC** operations.

For thread pool sizing involving **JDBC** connection pools, we want a fixed thread pool size matching the connection
pool, using a thread pool executor. We should configure our **JDBC** connection pool to double the number of physical
cores, plus the number of disk spindles, i.e. if we have a four core **CPU** and one disk, we have a total of 9 **JDBC**
connections in the pool:

```scala
# db connections = ((physical_core_count * 2) + effective_spindle_count)
fixedConnectionPool = 9

database.dispatcher {
  executor = "thread-pool-executor"
  throughput = 1
  thread-pool-executor {
    fixed-pool-size = ${fixedConnectionPool}
  }
}
```

### Configuring the connection pool
Out of the box, **Play** uses [**HikariCP**](https://github.com/brettwooldridge/HikariCP) as the default database
connection pool implementation. Also, we can use our own pool that implements `play.api.db.ConnectionPool` by specifying
the fully-qualified class name:

```ini
play.db.pool=our.own.ConnectionPool
```

The full range of configuration options for connection pools can be found by inspecting the `play.db.prototype` property
in **Play's JDBC** `reference.conf`.

## Compile-time dependency injection
Out of the box, **Play** provides a mechanism for runtime dependency injection - that is, dependency injection where
dependencies aren't wired until runtime. This approach has both advantages and disadvantages, the main advantages being
around minimization of boilerplate code, the main disadvantage being that the construction of the application is not
validated at compile time.

An alternative approach that is popular in **Scala** development is to use compile-time dependency injection. At its
simplest, compile-time **DI** can be achieved by manually constructing and wiring dependencies. Other more advanced
techniques and tools exist, such as macro-based autowiring tools, implicit auto wiring techniques, and various forms of
the cake pattern. All of these can be easily implemented on top of constructors and manual wiring, so **Play's** support
for compile-time dependency injection is provided by providing public constructors and factory methods as **API**.

In addition to providing public constructors and factory methods, all of **Play's** out of the box modules provide some
traits that implement a lightweight form of the cake pattern, for convenience. These are built on top of the public
constructors, and are completely optional. In some applications, they will not be appropriate to use, but in many
applications, they will be a very convenient mechanism to wiring the components provided by **Play**. These traits
follow a naming convention of ending the trait name with `Components`, so for example, the default **HikariCP**-based
implementation of the **DB API** provides a trait called `HikariCPComponents`.

### Application entry point
Every application that runs on the **JVM** needs an entry point that is loaded by reflection - even if our application
starts itself, the main class is still loaded by reflection, and its main method is located and invoked using
reflection.

In **Play's** dev mode, the **JVM** and **HTTP** server used by **Play** must be kept running between restarts of our
application. To implement this, **Play** provides an `ApplicationLoader` trait that we can implement. The application
loader is constructed and invoked every time the application is reloaded, to load the application.

This trait's load method takes as an argument the application loader `Context`, which contains all the components
required by a **Play** application that outlive the application itself and cannot be constructed by the application
itself. A number of these components exist specifically for the purposes of providing functionality in dev mode, for
example, the source mapper allows the **Play** error handlers to render the source code of the place that an exception
was thrown.

The simplest implementation of this can be provided by extending the **Play** `BuiltInComponentsFromContext` abstract
class. This class takes the context, and provides all the built-in components, based on that context. The only thing we
need to provide is a router for **Play** to route requests to. Below is the simplest application that can be created in
this way, using a null router:

```scala
import play.api._
import play.api.ApplicationLoader.Context
import play.api.routing.Router
import play.filters.HttpFiltersComponents

class MyApplicationLoader extends ApplicationLoader {
  def load(context: Context) = {
    new MyComponents(context).application
  }
}

class MyComponents(context: Context) extends BuiltInComponentsFromContext(context) with HttpFiltersComponents {
  lazy val router = Router.empty
}
```

To configure **Play** to use this application loader, we must configure the `play.application.loader` property to point
to the fully qualified class name in the `application.conf` file:

```ini
play.application.loader=MyApplicationLoader
```

In addition, if we're modifying an existing project that uses the built-in **Guice** module, we should be able to remove
`guice` from our `libraryDependencies` in `built.sbt`.

### Configuring logging
To correctly configure logging in **Play**, the `LoggerConfigurator` must be run before the application is returned. The
default `BuiltInComponentsFromContext` does not call `LoggerConfigurator` for us.

This initialization code must be added in our application loader:

```scala
class MyApplicationLoaderWithInitialization extends ApplicationLoader {
  def load(context: Context) = {
    LoggerConfigurator(context.environment.classLoader).foreach {
      _.configure(context.environment, context.initialConfiguration, Map.empty)
    }
    new MyComponents(context).application
  }
}
```

If we are migrating from **Play 2.4.x**, `LoggerConfigurator` is the replacement for `Logger.configure()` and allows for
customization of different logging frameworks.

### Providing a router
By default **Play** will use the injected routes generator. This generates a router with a constructor that accepts each
of the controllers and included routers from our routes file, in the order they appear in our routes file. The router's
consructor will also, as its first argument, accept an `HttpErrorHandler`, which is used to handle parameter binding
errors, and a prefix `String` as its last argument. An overloaded constructor that defaults this to `/` will also be
provided.

The following routes:

```ini
GET        /                    controllers.Application.index
GET        /foo                 controllers.Application.foo
->         /bar                 bar.Routes
GET        /assets/*file        controllers.Assets.at(path = "/public", file)
```

will produce a router with the following constructor signatures:

```scala
class Routes(
  override val errorHandler: play.api.http.HttpErrorHandler,
  Application_0: controllers.Application,
  bar_Routes_0: bar.Routes,
  Assets_1: controllers.Assets,
  val prefix: String
) extends GeneratedRouter {

  def this(
    errorHandler: play.api.http.HttpErrorHandler,
    Application_0: controllers.Application,
    bar_Routes_0: bar.Routes,
    Assets_1: controllers.Assets
  ) = this(Application_0, bar_Routes_0, Assets_1, "/")
  ...
}
```

Note that the naming of the parameters is intentionally not well defined (and in fact the index that is appended to them
is random, depending on hash map ordering), so we should not depend on the names of these parameters.

To use this router in an actual application:

```scala
import play.api._
import play.api.ApplicationLoader.Context
import play.filters.HttpFiltersComponents
import router.Routes

class MyApplicationLoader extends ApplicationLoader {
  def load(context: Context) = {
    new MyComponents(context).application
  }
}

class MyComponents(context: Context)
    extends BuiltInComponentsFromContext(context)
    with HttpFiltersComponents
    with controllers.AssetsComponents {
  lazy val barRoutes             = new bar.Routes(httpErrorHandler)
  lazy val applicationController = new controllers.Application(controllerComponents)

  lazy val router = new Routes(httpErrorHandler, applicationController, barRoutes, assets)
}
```

### Using other components
**Play** provides a number of helper traits for wiring in other components. For example, if we wanted to use the
messages module, we can mix in `I18nComponents` into our components cake, like so:

```scala
import play.api.i18n._

class MyComponents(context: Context)
    extends BuiltInComponentsFromContext(context)
    with I18nComponents
    with HttpFiltersComponents {
  lazy val router = Router.empty

  lazy val myComponent = new MyComponent(messagesApi)
}

class MyComponent(messages: MessagesApi) {
  // ...
}
```

Other helper traits are also available, such as the `CSRFComponents` or the `AhcWSComponents`.

## The Play JSON library
The `play.api.libs.json` package contains data structures for representing **JSON** data and utilities for converting
between these data structures and other data representations. Some of the features of this package are:

- Automatic conversion to and from case classes with minimal boilerplate. If we want to get up and running quickly with
  minimal code, this is probably the place to start.

- Custom validation while parsing.

- Automatic parsing of **JSON** in request bodies, with auto-generated errors if content isn't parseable or incorrect
  `Content-Type` headers are supplied.

- Can be used outside of a **Play** application as a standalone library. We just add `libraryDependencies +=
  "com.typesafe.play" %% "play-json" % playVersion` to our `build.sbt` file.

- Highly customizable.

The package provides the following types:

- ### `JsValue`
  This is a trait representing any **JSON** value. The **JSON** library has a case class extending `JsValue` to
  represent each valid **JSON** type:

  - `JsString`
  - `JsNumber`
  - `JsBoolean`
  - `JsObject`
  - `JsArray`
  - `JsNull`

  <br>

  Using the various `JsValue` types, we can construct a representation of any **JSON** structure.

- ### `Json`
  The `Json` object provides utilities, primarily for conversion to and from `JsValue` structures.

- ### `JsPath`
  Represents a path into a `JsValue` structure, analogous to `XPath` for `XML`. This is used for traversing `JsValue`
  structures and in patterns for implicit converters.

### Converting to a `JsValue`
#### Using string parsing
```scala
import play.api.libs.json._

val json: JsValue = Json.parse("""
  {
    "name" : "Watership Down",
    "location" : {
      "lat" : 51.235685,
      "long" : -1.309197
    },
    "residents" : [ {
      "name" : "Fiver",
      "age" : 4,
      "role" : null
    }, {
      "name" : "Bigwig",
      "age" : 6,
      "role" : "Owsla"
    } ]
  }
  """)
```

#### Using class construction
```scala
import play.api.libs.json._

val json: JsValue = JsObject(
  Seq(
    "name"     -> JsString("Watership Down"),
    "location" -> JsObject(Seq("lat" -> JsNumber(51.235685), "long" -> JsNumber(-1.309197))),
    "residents" -> JsArray(
      IndexedSeq(
        JsObject(
          Seq(
            "name" -> JsString("Fiver"),
            "age"  -> JsNumber(4),
            "role" -> JsNull
          )
        ),
        JsObject(
          Seq(
            "name" -> JsString("Bigwig"),
            "age"  -> JsNumber(6),
            "role" -> JsString("Owsla")
          )
        )
      )
    )
  )
)
```

`Json.obj` and `Json.arr` can simplify construction a bit. Note that most values don't need to be explicitly wrapped by
`JsValue` classes, the factory methods use implicit conversion (more on this below).

```scala
import play.api.libs.json.JsNull
import play.api.libs.json.Json
import play.api.libs.json.JsString
import play.api.libs.json.JsValue

val json: JsValue = Json.obj(
  "name"     -> "Watership Down",
  "location" -> Json.obj("lat" -> 51.235685, "long" -> -1.309197),
  "residents" -> Json.arr(
    Json.obj(
      "name" -> "Fiver",
      "age"  -> 4,
      "role" -> JsNull
    ),
    Json.obj(
      "name" -> "Bigwig",
      "age"  -> 6,
      "role" -> "Owsla"
    )
  )
)
```

#### Using Writes converters
**Scala** to `JsValue` conversion is performed by the utility method `Json.toJson[T](T)(implicit writes: Writes[T])`.
This functionality depends on a converter of type `Writes[T]` which can convert a `T` to a `JsValue`.
The **Play JSON API** provides implicit `Writes` for most basic types, such as `Int`, `Double`, `String`, and `Boolean`.
It also supports `Writes` for collections of any type `T` that a `Writes[T]` exists.

```scala
import play.api.libs.json._

// basic types
val jsonString  = Json.toJson("Fiver")
val jsonNumber  = Json.toJson(4)
val jsonBoolean = Json.toJson(false)

// collections of basic types
val jsonArrayOfInts    = Json.toJson(Seq(1, 2, 3, 4))
val jsonArrayOfStrings = Json.toJson(List("Fiver", "Bigwig"))
```

To convert our own models to `JsValue`s, we must define implicit `Writes` converters and provide them in scope.

```scala
case class Location(lat: Double, long: Double)
case class Resident(name: String, age: Int, role: Option[String])
case class Place(name: String, location: Location, residents: Seq[Resident])
```

```scala
import play.api.libs.json._

implicit val locationWrites = new Writes[Location] {
  def writes(location: Location) = Json.obj(
    "lat"  -> location.lat,
    "long" -> location.long
  )
}

implicit val residentWrites = new Writes[Resident] {
  def writes(resident: Resident) = Json.obj(
    "name" -> resident.name,
    "age"  -> resident.age,
    "role" -> resident.role
  )
}

implicit val placeWrites = new Writes[Place] {
  def writes(place: Place) = Json.obj(
    "name"      -> place.name,
    "location"  -> place.location,
    "residents" -> place.residents
  )
}

val place = Place(
  "Watership Down",
  Location(51.235685, -1.309197),
  Seq(
    Resident("Fiver", 4, None),
    Resident("Bigwig", 6, Some("Owsla"))
  )
)

val json = Json.toJson(place)
```

Alternatively, we can define our `Writes` using the combinator pattern:

```scala
import play.api.libs.json._
import play.api.libs.functional.syntax._

implicit val locationWrites: Writes[Location] = (
  (JsPath \ "lat").write[Double] and
    (JsPath \ "long").write[Double]
)(unlift(Location.unapply))

implicit val residentWrites: Writes[Resident] = (
  (JsPath \ "name").write[String] and
    (JsPath \ "age").write[Int] and
    (JsPath \ "role").writeNullable[String]
)(unlift(Resident.unapply))

implicit val placeWrites: Writes[Place] = (
  (JsPath \ "name").write[String] and
    (JsPath \ "location").write[Location] and
    (JsPath \ "residents").write[Seq[Resident]]
)(unlift(Place.unapply))
```

### Traversing a `JsValue` structure
We can traverse a `JsValue` structure and extract specific values. The syntax and functionality is similar to **Scala
XML** processing.

#### Simple path `\`
Applying the `\` operator to a `JsValue` will return the property corresponding to the field argument in a `JsObject`,
or the item at that index in a `JsArray`.

```scala
val lat = (json \ "location" \ "lat").get
// returns JsNumber(51.235685)
val bigwig = (json \ "residents" \ 1).get
// returns {"name":"Bigwig","age":6,"role":"Owsla"}
```

The `\` operator returns a `JsLookupResult` which is either `JsDefined` or `JsUndefined`. We can chain multiple `\`
operators, and the result will be `JsUndefined` if any intermediate value cannot be found. Calling `get` on a
`JsLookupResult` attempts to get the value if it is defined and throws an exception if it is not.

We can also use the [Direct lookup](#direct-lookup) `apply` method (below) to get a field in an object or index in an
array. Like `get`, this method will throw an exception if the value does not exist.

#### Recursive path `\\`
Applying the `\\` operator will do a lookup for the field in the current object and all descendants.

```scala
val names = json \\ "name"
// returns Seq(JsString("Watership Down"), JsString("Fiver"), JsString("Bigwig"))
```

#### Direct lookup
We can retrieve a value in a `JsArray` or `JsObject` using an `apply` operator, which is identical to the [Simple path
`\`](#simple-path-%5C) operator except it returns the value directly (rather than wrapping it in a `JsLookupResult`) and
throws an exception if the index or key is not found:

```scala
val name = json("name")
// returns JsString("Watership Down")

val bigwig2 = json("residents")(1)
// returns {"name":"Bigwig","age":6,"role":"Owsla"}

// (json("residents")(3)
// throws an IndexOutOfBoundsException

// json("bogus")
// throws a NoSuchElementException
```

This is useful if we are writing quick-and-dirty code and are accessing some **JSON** values we know to exist, for
example in one-off scripts or in the **REPL**.

### Converting from a `JsValue`
#### Using `String` utilities
Minified:

```scala
val minifiedString: String = Json.stringify(json)
```

Readable:

```scala
val readableString: String = Json.prettyPrint(json)
```

```json
{
  "name" : "Watership Down",
  "location" : {
    "lat" : 51.235685,
    "long" : -1.309197
  },
  "residents" : [ {
    "name" : "Fiver",
    "age" : 4,
    "role" : null
  }, {
    "name" : "Bigwig",
    "age" : 6,
    "role" : "Owsla"
  } ]
}
```

#### Using `JsValue.as/asOpt`
The simplest way to convert a `JsValue` to another type is using `JsValue.as[T](implicit fjs: Reads[T]): T`. This
requires an implicit converter of type `Reads[T]` to convert a `JsValue` to `T` (the inverse of `Writes[T]`). As with
`Writes`, the **JSON API** provides `Reads` for basic types.

```scala
val name = (json \ "name").as[String]
// "Watership Down"

val names = (json \\ "name").map(_.as[String])
// Seq("Watership Down", "Fiver", "Bigwig")
```

The `as` method will throw a `JsResultException` if the path is not found or the conversion is not possible. A safer
method is `JsValue.asOpt[T](implicit fjs: Reads[T]): Option[T]`.

```scala
val nameOption = (json \ "name").asOpt[String]
// Some("Watership Down")

val bogusOption = (json \ "bogus").asOpt[String]
// None
```

Although the `asOpt` method is safer, any error information is lost.

#### Using validation
The preferred way to convert from a `JsValue` to another type is by using its `validate` method (which takes an argument
of type `Reads`). This performs both validation and conversion, returning a type of `JsResult`. `JsResult` is
implemented by two classes:

- `JsSuccess`: Represents a successful validation/conversion and wraps the result.
- `JsError`: Represents unsuccessful validation/conversion and contains a list of validation errors.

We can apply various patterns for handling a validation result:

```scala
val json = { ... }

val nameResult: JsResult[String] = (json \ "name").validate[String]

// Pattern matching
nameResult match {
  case JsSuccess(name, _) => println(s"Name: $name")
  case e: JsError         => println(s"Errors: ${JsError.toJson(e)}")
}

// Fallback value
val nameOrFallback = nameResult.getOrElse("Undefined")

// map
val nameUpperResult: JsResult[String] = nameResult.map(_.toUpperCase)

// fold
val nameOption: Option[String] = nameResult.fold(
  invalid = { fieldErrors =>
    fieldErrors.foreach { x =>
      println(s"field: ${x._1}, errors: ${x._2}")
    }
    Option.empty[String]
  },
  valid = Some(_)
)
```

#### `JsValue` to a model
To convert from `JsValue` to a model, we must define implicit `Reads[T]` where `T` is the type of our model.

```scala
case class Location(lat: Double, long: Double)
case class Resident(name: String, age: Int, role: Option[String])
case class Place(name: String, location: Location, residents: Seq[Resident])
```

```scala
import play.api.libs.json._
import play.api.libs.functional.syntax._

implicit val locationReads: Reads[Location] = (
  (JsPath \ "lat").read[Double] and
    (JsPath \ "long").read[Double]
)(Location.apply _)

implicit val residentReads: Reads[Resident] = (
  (JsPath \ "name").read[String] and
    (JsPath \ "age").read[Int] and
    (JsPath \ "role").readNullable[String]
)(Resident.apply _)

implicit val placeReads: Reads[Place] = (
  (JsPath \ "name").read[String] and
    (JsPath \ "location").read[Location] and
    (JsPath \ "residents").read[Seq[Resident]]
)(Place.apply _)

val json = { ... }

val placeResult: JsResult[Place] = json.validate[Place]
// JsSuccess(Place(...),)

val residentResult: JsResult[Resident] = (json \ "residents")(1).validate[Resident]
// JsSuccess(Resident(Bigwig,6,Some(Owsla)),)
```

## JSON with HTTP
**Play** supports **HTTP** requests and responses with a content type of **JSON** by using the **HTTP API** in
combination with the **JSON** library.

We’ll demonstrate the necessary concepts by designing a simple **RESTful** web service to `GET` a list of entities and
accept `POST`s to create new entities. The service will use a content type of **JSON** for all data.

Here’s the model we’ll use for our service:

```scala
case class Location(lat: Double, long: Double)

case class Place(name: String, location: Location)

object Place {
  var list: List[Place] = {
    List(
      Place(
        "Sandleford",
        Location(51.377797, -1.318965)
      ),
      Place(
        "Watership Down",
        Location(51.235685, -1.309197)
      )
    )
  }

  def save(place: Place) = {
    list = list ::: List(place)
  }
}
```

### Serving a list of entities in JSON
We'll start by adding the necessary imports to our controller.

```scala
import play.api.mvc._

class HomeController @Inject() (cc: ControllerComponents) extends AbstractController(cc) {}
```

Before we write our `Action`, we'll need the plumbing for doing conversion from our model to a `JsValue` representation.
This is accomplished by defining an implicit `Writes[Place]`.

```scala
implicit val locationWrites: Writes[Location] =
  (JsPath \ "lat").write[Double].and((JsPath \ "long").write[Double])(unlift(Location.unapply))

implicit val placeWrites: Writes[Place] =
  (JsPath \ "name").write[String].and((JsPath \ "location").write[Location])(unlift(Place.unapply))
```

Next we write our `Action`:

```scala
def listPlaces = Action {
  val json = Json.toJson(Place.list)
  Ok(json)
}
```

The `Action` retrieves a list of `Place` objects, converts them to a `JsValue` using `Json.toJson` with our implicit
`Writes[Place]`, and returns this as the body of the result. **Play** will recognize the result as **JSON** and set the
appropriate `Content-Type` header and body value for the response.

The last step is to add a route for our `Action` in `conf/routes`:

```ini
GET   /places               controllers.Application.listPlaces
```

We can test the action by making a request with a browser or **HTTP** tool. This example uses the **UNIX** command line
tool [**cURL**](https://curl.se/).

```sh
curl --include http://localhost:9000/places
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 141

[{"name":"Sandleford","location":{"lat":51.377797,"long":-1.318965}},{"name":"Watership Down","location":{"lat":51.235685,"long":-1.309197}}]
```

### Creating a new entity instance in JSON
For this `Action` we'll need to define an implicit `Reads[Place]` to convert a `JsValue` to our model.

```scala
implicit val locationReads: Reads[Location] =
  (JsPath \ "lat").read[Double].and((JsPath \ "long").read[Double])(Location.apply _)

implicit val placeReads: Reads[Place] =
  (JsPath \ "name").read[String].and((JsPath \ "location").read[Location])(Place.apply _)
```

Next we'll define the `Action`:

```scala
def savePlace = Action(parse.json) { request =>
  val placeResult = request.body.validate[Place]
  placeResult.fold(
    errors => {
      BadRequest(Json.obj("message" -> JsError.toJson(errors)))
    },
    place => {
      Place.save(place)
      Ok(Json.obj("message" -> ("Place '" + place.name + "' saved.")))
    }
  )
}
```

This `Action` is more complicated than our list case. Some things to note:

- This `Action` expects a request with a `Content-Type` header of `text/json` or `application/json` and a body
  containing a **JSON** representation of the entity to create.

- It uses a **JSON**-specific `BodyParser` which will parse the request and provide `request.body` as a `JsValue`.

- We used the `validate` method for conversion which will rely on our implicit `Reads[Place]`.

- To process the validation result, we used a `fold` with error and success flows. This pattern may be familiar as it is
  also used for form submission.

- This `Action` also sends **JSON** responses.

Body parsers can be typed with a case class, an explicit `Reads` object or take a function. So we can offload even more
of the work onto **Play** to make it automatically parse **JSON** to a case class and validate it before even calling
our `Action`:

```scala
import play.api.libs.functional.syntax._
import play.api.libs.json.Reads._
import play.api.libs.json._

implicit val locationReads: Reads[Location] = (
  (JsPath \ "lat")
    .read[Double](min(-90.0).keepAnd(max(90.0)))
    .and((JsPath \ "long").read[Double](min(-180.0).keepAnd(max(180.0))))
  )(Location.apply _)

implicit val placeReads: Reads[Place] =
  (JsPath \ "name").read[String](minLength[String](2)).and((JsPath \ "location").read[Location])(Place.apply _)

// This helper parses and validates JSON using the implicit `placeReads`
// above, returning errors if the parsed json fails validation.
def validateJson[A: Reads] = parse.json.validate(
  _.validate[A].asEither.left.map(e => BadRequest(JsError.toJson(e)))
)

// if we don't care about validation we could replace `validateJson[Place]`
// with `BodyParsers.parse.json[Place]` to get an unvalidated case class
// in `request.body` instead.
def savePlaceConcise = Action(validateJson[Place]) { request =>
  // `request.body` contains a fully validated `Place` instance.
  val place = request.body
  Place.save(place)
  Ok(Json.obj("message" -> ("Place '" + place.name + "' saved.")))
}
```

Finally we'll add a route binding in `conf/routes`:

```ini
POST  /places               controllers.Application.savePlace
```

We'll test this action with valid and invalid requests to verify our success and error flows.

Testing the action with valid data:

```sh
curl --include
  --request POST
  --header "Content-type: application/json" 
  --data '{"name":"Nuthanger Farm","location":{"lat" : 51.244031,"long" : -1.263224}}' 
  http://localhost:9000/places
```

Response:

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 57

{"message":"Place 'Nuthanger Farm' saved."}
```

Testing the action with invalid data, missing "name" field:

```sh
curl --include
  --request POST
  --header "Content-type: application/json"
  --data '{"location":{"lat" : 51.244031,"long" : -1.263224}}' 
  http://localhost:9000/places
```

Response:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json
Content-Length: 79

{"message":{"obj.name":[{"msg":"error.path.missing","args":[]}]}}
```

Testing the action with invalid data, wrong data type for "lat":

```sh
curl --include
  --request POST
  --header "Content-type: application/json" 
  --data '{"name":"Nuthanger Farm","location":{"lat" : "xxx","long" : -1.263224}}' 
  http://localhost:9000/places
```

Response:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json
Content-Length: 92

{"message":{"obj.location.lat":[{"msg":"error.expected.jsnumber","args":[]}]}}
```

### Summary
**Play** is designed to support **REST** with **JSON** and developing these services should hopefully be
straightforward. The bulk of the work is in writing `Reads` and `Writes` for our model.
