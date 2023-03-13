# Xamarin.Forms

## General
- **Xamarin** was founded by the engineers who created the Mono Project
- It is now a subsidiary of Microsoft
- **Xamarin.Forms** is built on top of **Xamarin**
- It enables the development of cross-platform mobile apps
- Views are defined in **XAML** (`.xaml`) files with code-behind in **C#** (`.xaml.cs`)

## Controls
Below are some of the available UI elements:
- **Label:** Displays single-line text strings or multi-line blocks of text, either with constant or variable formatting
- **Entry:** A view used for single-line text input
- **Button:** A rectangular object that displays text, and which fires a `Clicked` event when it's been pressed
- **ImageButton:** Displays an image and responds to a tap or click that directs an application to carry out a particular task
- **ListView:** A view for presenting lists of data, especially long lists that require scrolling
- **FlexLayout:** A layout for stacking or wrapping a collection of child views based on the **CSS Flexible Box Layout Module**
- **Frame:** A layout used to wrap a view with a border that can be configured with color, shadow, and other options

Each control supports its own set of properties.

The `x:Name` directive can be used to uniquely identify **XAML**-defined elements in a **XAML** namescope and is useful for referring to those elements from **C#**.

## MVVM
The *Model-View-ViewModel* (**MVVM**) pattern helps to cleanly separate the business and presentation logic of an application from its user interface (UI).
Maintaining a clean separation between application logic and the UI helps to address numerous development issues and can make an application easier to test,
maintain, and evolve. It can also greatly improve code re-use opportunities and allows developers and UI designers to more easily collaborate when developing
their respective parts of an app.

There are three core components in the **MVVM** pattern: the model, the view, and the view model.

Each serves a distinct purpose:
- The view is responsible for defining the structure, layout, and appearance of what the user sees on screen. Ideally, each view is defined in **XAML**, with a
  limited code-behind that does not contain business logic. However, in some cases, the code-behind might contain UI logic that implements visual behavior that is
  difficult to express in **XAML**, such as animations.
- The view model implements properties and commands to which the view can data bind to, and notifies the view of any state changes through change notification events.
  The properties and commands that the view model provides define the functionality to be offered by the UI, but the view determines how that functionality is to be
  displayed. The view model is also responsible for coordinating the view's interactions with any model classes that are required. There's typically a one-to-many
  relationship between the view model and the model classes. In order for the view model to participate in two-way data binding with the view, its properties must raise
  the `PropertyChanged` event. View models satisfy this requirement by implementing the `INotifyPropertyChanged` interface, and raising the `PropertyChanged` event when
  a property is changed. For collections, the view-friendly `ObservableCollection<T>` is provided. This collection implements collection changed notification, relieving
  the developer from having to implement the `INotifyCollectionChanged` interface on collections.
- Model classes are non-visual classes that encapsulate the app's data. Therefore, the model can be thought of as representing the app's domain model, which usually
  includes a data model along with business and validation logic. Examples of model objects include *data transfer objects* (**DTO**s), *Plain Old CLR Objects* (**POCO**s),
  and generated entity and proxy objects.

At a high level, the view "knows about" the view model, and the view model "knows about" the model, but the model is unaware of the view model, and the view model
is unaware of the view. Therefore, the view model isolates the view from the model, and allows the model to evolve independently of the view.

The benefits of using the **MVVM** pattern are as follows:
- If there's an existing model implementation that encapsulates existing business logic, it can be difficult or risky to change it. In this scenario, the view model
  acts as an adapter for the model classes and enables you to avoid making any major changes to the model code.
- Developers can create unit tests for the view model and the model, without using the view. The unit tests for the view model can exercise exactly the same functionality
  as used by the view.
- The app UI can be redesigned without touching the code, provided that the view is implemented entirely in **XAML**. Therefore, a new version of the view should work with
  the existing view model.
- Designers and developers can work independently and concurrently on their components during the development process. Designers can focus on the view, while developers can
  work on the view model and model components.

## Connecting View Models to Views
The aim is for the view to have a view model assigned to its `BindingContext` property.

The simplest approach is for the view to declaratively instantiate its corresponding view model in **XAML**. When the view is constructed, the corresponding view model object
will also be constructed. This approach is demonstrated in the following code example:

```xml
<ContentPage ... xmlns:local="clr-namespace:eShop">
    <ContentPage.BindingContext>
        <local:LoginViewModel />
    </ContentPage.BindingContext>
    ...
</ContentPage>
```

When the `ContentPage` is created, an instance of the `LoginViewModel` is automatically constructed and set as the view's `BindingContext`.

This declarative construction and assignment of the view model by the view has the advantage that it's simple, but has the disadvantage that it requires a default
(parameter-less) constructor in the view model.

Alternatively, a view can have code in the code-behind file that results in the view model being assigned to its `BindingContext` property. This is often accomplished in the
view's constructor, as shown in the following code example:

```cs
public LoginView()
{
    InitializeComponent();
    BindingContext = new LoginViewModel(navigationService);
}
```

The programmatic construction and assignment of the view model within the view's code-behind has the advantage that it's simple. However, the main disadvantage of this approach
is that the view needs to provide the view model with any required dependencies. Using a dependency injection container can help to maintain loose coupling between the view and
view model.

## Implementing Commands
View models typically expose command properties, for binding from the view, that are object instances that implement the `ICommand` interface. A number of **Xamarin.Forms**
controls provide a `Command` property, which can be data bound to an `ICommand` object provided by the view model. The `ICommand` interface defines an `Execute` method, which
encapsulates the operation itself, a `CanExecute` method, which indicates whether the command can be invoked, and a `CanExecuteChanged` event that occurs when changes occur that
affect whether the command should execute. The `Command` and `Command<T>` classes, provided by **Xamarin.Forms**, implement the `ICommand` interface, where `T` is the type of the
arguments to `Execute` and `CanExecute`.

Within a view model, there should be an object of type `Command` or `Command<T>` for each public property in the view model of type `ICommand`. The `Command` or `Command<T>`
constructor requires an `Action` callback object that's called when the `ICommand.Execute` method is invoked. The `CanExecute` method is an optional constructor parameter, and is
a `Func` that returns a `bool`.

The following code shows how a `Command` instance, which represents a register command, is constructed by specifying a delegate to the `Register` view model method:

```cs
public ICommand RegisterCommand => new Command(Register);
```

The command is exposed to the view through a property that returns a reference to an `ICommand`. When the `Execute` method is called on the `Command` object, it simply forwards
the call to the method in the view model via the delegate that was specified in the `Command` constructor.

Parameters can be passed to the `Execute` and `CanExecute` actions by using the `Command<T>` class to instantiate the command. For example, the following code shows how a
`Command<T>` instance is used to indicate that the `NavigateAsync` method will require an argument of type `string`:

```cs
public ICommand NavigateCommand => new Command<string>(NavigateAsync);
```

In both the `Command` and `Command<T>` classes, the delegate to the `CanExecute` method in each constructor is optional. If a delegate isn't specified, the `Command` will return
`true` for `CanExecute`. However, the view model can indicate a change in the command's `CanExecute` status by calling the `ChangeCanExecute` method on the `Command` object. This
causes the `CanExecuteChanged` event to be raised. Any controls in the UI that are bound to the command will then update their enabled status to reflect the availability of the
data-bound command.

## ListView
```xml
<ListView BackgroundColor="Transparent">
    <ListView.ItemsSource>
        <x:Array Type="{x:Type x:String}">
            <x:String>Item One</x:String>
            <x:String>Item Two</x:String>
            <x:String>Item Three</x:String>
        </x:Array>
    </ListView.ItemsSource>
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="model:Coffee">
            <TextCell Text="{Binding}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

- `ScrollView` capabilities already built-in
- Default `ViewCell` just uses `ToString` on the contents to display them
- `ListView.ItemsSource` is the source of items displayed in the `ListView`
- `ListView.ItemTemplate` allows us to provide a custom `ViewCell` and data bind to properties of the item inside it
- Every `ListView.ItemTemplate` is a `DataTemplate`
- We can put any `ViewCell` inside the `DataTemplate`:
  - A `TextCell` with `Text` and `Detail` properties
  - A `SwitchCell` with a `Text` property combined with a switch element
  - An `ImageCell` with `Text`, `Detail` and `ImageSource` properties, or
  - A `ViewCell` when we want full control over customization
- The item height is fixed by default but we can either set the `RowHeight` directly or set the `HasUnevenRows` property to `True`
- Can customize the separator between items with the `SeparatorColor` and `SeparatorVisibility` properties
- `ListView.Header` allows us to customize the UI above the items (e.g. put a title)
- `ListView.Footer` allows us to customize the UI below the items (e.g. put a button)
- `ViewCell.ContextActions` can be used to create `MenuItem`s that are shown when long pressing (**Android**) or swiping left or right (**iOS**) on an item
- Can bind a list of lists as the `ItemsSource` and show it by setting `IsGroupingEnabled` to `True` and binding `GroupDisplayBinding` to an object that will be used as the
  group header
- `ListView.GroupHeaderTemplate` allows for more detailed customization of the group headers
- Refreshing behavior can be controlled with the `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshCommand` and `RefreshControlColor` properties
- Set `CachingStrategy` to `RecycleElement` for better optimization behavior
- The `ItemSelected` event is triggered when the selection changes from one item to another and keeps the item selected until it is manually deselected
- The `ItemTapped` event is triggered every time an item is tapped, and after the `ItemSelected` event, if both are triggered
- Can access the selected item from **C#** by using the `SelectedItem` property

## DataTemplate
A template for multiple bindings, commonly used by `ListView`s and `MultiPage<T>`s.

In **XAML**, application developers can nest markup inside a `DataTemplate` tag to create a view whose members are bound to the properties of data objects that are contained in a
`ItemsSource` list. For example:

```xml
<ListView x:Name="TodoList" ItemsSource="{Binding TodoItems}">
  <ListView.ItemTemplate>
    <DataTemplate>
      <ViewCell>
        <Label Text="{Binding TodoText}" FontSize="Large" />
      </ViewCell>
    </DataTemplate>
  </ListView.ItemTemplate>
</ListView>
```
