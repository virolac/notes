# ASP.NET

## Table of Contents
- [General](#general)
- [Project structure](#project-structure)
  - [wwwroot](#wwwroot)
  - [Models](#models)
  - [Views](#views)
  - [Controllers](#controllers)
  - [Program.cs](#program.cs)
  - [appsettings.json](#appsettings.json)
- [Routing](#routing)
- [Passing model objects to views](#passing-model-objects-to-views)
- [Tag helpers](#tag-helpers)
- [Entity Framework](#entity-framework)
- [Validation example](#validation-example)

## General
- A **M**odel-**V**iew-**C**ontroller (**MVC**) web framework
  - `Model` - represents the shape of the data
  - `View` - represents the user interface
  - `Controller` - handles user requests and acts as an interface between the `Model` and the `View`
- Uses the **Razor** syntax for writing view templates
- **Razor** is based on **C#** and uses the `@` character to add code to a page
- Code blocks are enclosed in braces:
  ```html
  @{
    var outsideTemp = 79;
    var weatherMessage = "Hello, it is " + outsideTemp + " degrees.";
  }
  <p>Today's weather: @weatherMessage</p>
  ```

## Project structure
- ### wwwroot
  Stores static files for the project (e.g. `.css`, `.js`, etc.).

- ### Models
  Stores **POCO** (**P**lain-**O**ld **C**LR **O**bjects) classes for the project.

- ### Views
  Stores `.cshtml` templates in different folders, one for each controller. 

  The views belonging to the `HomeController` are stored in a folder named `Home`, those belonging to the
  `AboutController` are stored in a folder named `About`, etc.

  The `Shared` folder stores **partial views** that are shared among multiple pages in the application. Partial view
  names begin with an underscore (`_`).

  The `_Layout.cshtml` partial view is special, in that it is the default master page of the application.

  The `_ViewImports.cshtml` file is used for global definition of namespace aliases and for importing **Microsoft**'s
  [tag helpers](#tag-helpers) (e.g. `asp-controller`).

  The `_ViewStart.cshtml` file defines the default master page for the application (usually `_Layout.cshtml`).

- ### Controllers
  Stores the application controllers that define and group a set of actions for handling requests.

  **ASP.NET Core** provides the following options for web **API** controller action return types:

  - **Specific type**  
    A primitive or complex data type, such as `string` or a custom object.
  - **IActionResult**  
    Appropriate when multiple `ActionResult` return types are possible in an action. The `ActionResult` types represent
    various **HTTP** status codes, e.g. `BadRequestResult` (**400**), `NotFoundResult` (**404**), etc.
  - **ActionResult<T>**  
    Enables returning a type deriving from `ActionResult` or returning a specific type. The action's expected return
    type is inferred from the `T` in `ActionResult<T>`.
  - **HttpResults**  
    Can be useful when sharing code between [**Minimal APIs**](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis) 
    and **Web API**.

- ### Program.cs
  Contains the application startup code where services required by the app are configured and where the request handling
  pipeline is defined.

  The request handling pipeline is composed as a series of *middleware components*. Each component performs operations
  on an `HttpContext` and either invokes the next middleware in the pipeline or terminates the request.

  By convention, a *middleware component* is added to the pipeline by invoking a `Use{Feature}` extension method.

  **Example:**

  ```cs
  var builder = WebApplication.CreateBuilder(args);

  // Add services to the container.
  builder.Services.AddControllersWithViews();

  var app = builder.Build();

  // Configure the HTTP request pipeline.
  if (app.Environment.IsDevelopment()) {
      app.UseDeveloperExceptionPage();
  } else {
      app.UseExceptionHandler("/Home/Error");
      // The default HSTS value is 30 days. You may want to change this for production.
      app.UseHsts();
  }

  app.UseHttpsRedirection();
  app.UseStaticFiles();

  app.UseRouting();

  app.UseAuthorization();

  app.MapControllerRoute(
      name: "default",
      pattern: "{controller=Home}/{action=Index}/{id?}"
  );

  app.Run();
  ```

  The order of middleware in the pipeline is important (e.g. `app.UseAuthentication()` must come before `app.UseAuthorization()`).

- ### appsettings.json
  Contains the application settings.

  We can also create per-environment settings in a file called `appsettings.{Environment}.json` (e.g.
  `appsettings.Development.json`).

## Routing
The `UseRouting` *middleware* adds route matching to the pipeline. This *middleware* looks at the set of endpoints
defined in the app, and selects the [**best match**](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/routing?view=aspnetcore-7.0#urlm) 
based on the request.

**URL** *pattern matching* works with paths of the form `/{controller}/{action}/{id?}`.

For example, a request for `/Users/Edit/1` will run the `Edit` action in the `UsersController`, passing `1` as the
parameter:

```cs
public class UsersController : Controller {
    public IActionResult Edit(int id) {
       return View();
    }
}
```

The `View()` call shown above will search in `Views/Users` for a view having the same name as the action (i.e. `Edit.cshtml`).

## Passing model objects to views
Controller action methods which return an `ActionResult` can pass a model object to the view. This allows a
**Controller** to cleanly package up all the information needed to generate a response, and then pass this information
off to a **View** template to use to generate the appropriate **HTML** response.

In the controller:

```cs
public IActionResult Categories()
{
    IEnumerable<Category> categories = ...;
    return View(categories);
}
```

In the view template:

```html
@model IEnumerable<Category>
<table>
  <thead>
    <tr>
      <th>Category Name</th>
      <th>Display Order</th>
    </tr>
  </thead>
  <tbody>
    @foreach (var obj in Model) {
      <tr>
        <td>@obj.Name</td>
        <td>@obj.DisplayOrder</td>
      </tr>
    }
  </tbody>
</table>
```

## Tag helpers
**Tag Helpers** enable server-side code to participate in creating and rendering **HTML** elements in **Razor** files.

**Examples:**
-  ```html
   <label asp-for="Movie.Title"></label>
   ```

- ```html
  <a asp-controller="Speaker"
     asp-action="Detail" 
     asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
  ```

## Entity Framework
The *Entity Framework* (**EF**) is an *object-relational mapper* (**O/RM**) that enables **.NET** developers to work with
relational data using domain-specific objects.

It uses special annotations for model properties:

- `[Key]` -  decorates the primary key
- `[Required]` - decorates a **NOT NULL** property

We can combine these annotations with attributes provided by the `System.ComponentModel` namespace:

- `[DisplayName("<name>")]` - specifies the display name for a property
- `[Range(min, max)]` - specifies the numeric range constraints for a property

## Validation example
**Controller:**

```cs
[HttpPost]
[ValidateAntiForgeryToken]
public IActionResult Create(Category category)
{
    if (category.Name == category.DisplayOrder.ToString()) {
        ModelState.AddModelError("name", "The DisplayOrder cannot exactly match the Name.");
    }

    if (ModelState.IsValid) {
        _db.Categories.Add(category);
        _db.SaveChanges();
        TempData["success"] = "Category created successfully!";
        return RedirectToAction("Index");
    }

    return View(category);
}
```

**View:**

```html
<h4>Movie</h4>
<hr />
<div class="row">
  <div class="col-md-4">
    <form asp-action="Create">
      <div asp-validation-summary="ModelOnly" class="text-danger"></div>
      <div class="form-group">
        <label asp-for="Title" class="control-label"></label>
        <input asp-for="Title" class="form-control" />
        <span asp-validation-for="Title" class="text-danger"></span>
      </div>
      ...
    </form>
  </div>
</div>

@section Scripts {
  @{
    <partial name="_ValidationScriptsPartial" />
  }
}
```
