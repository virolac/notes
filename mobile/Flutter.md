# Flutter

## Table of Contents
- [What is Flutter?](#what-is-flutter%3F)
- [The Dart Programming Language](#the-dart-programming-language)
- [Installation](#installation)
- [Creating an app](#creating-an-app)
- [Widgets](#widgets)
  - [Stateless widgets](#stateless-widgets)
  - [Stateful widgets](#stateful-widgets)
  - [Built-in widgets](#built-in-widgets)
    - [MaterialApp](#materialapp)
    - [Scaffold](#scaffold)
    - [FloatingActionButton](#floatingactionbutton)
    - [AppBar](#appbar)
    - [NavigationBar](#navigationbar)
    - [NavigationDestination](#navigationdestination)
    - [Navigator](#navigator)
    - [MaterialPageRoute](#materialpageroute)
    - [ThemeData](#themedata)
    - [Text](#text)
    - [TextStyle](#textstyle)
    - [ListView](#listview)
    - [ListTile](#listtile)
    - [Center](#center)
    - [Row](#row)
    - [Column](#column)
    - [Container](#container)
    - [Divider](#divider)
    - [Image](#image)
    - [Icon](#icon)
    - [IconButton](#iconbutton)
    - [ElevatedButton](#elevatedbutton)
    - [OutlinedButton](#outlinedbutton)
    - [TextButton](#textbutton)
    - [SizedBox](#sizedbox)
    - [GestureDetector](#gesturedetector)
    - [Switch](#switch)
    - [Checkbox](#checkbox)
    - [SingleChildScrollView](#singlechildscrollview)
- [Utility classes](#utility-classes)
  - [Colors](#colors)
  - [Icons](#icons)
  - [EdgeInsets](#edgeinsets)

## What is Flutter?
- Google UI toolkit for building natively compiled apps for mobile,
  web and desktop
- Build **iOS** and **Android** apps using a single codebase
- Cross platform - **Mac** / **Windows** / **Linux**
- Extremely fast

## The Dart Programming Language
- **Flutter** uses an **OOP** language called **Dart**
- Optimized for UI
- Fast on all platforms
- Similar to **JavaScript** with elements of **Java**

## Installation
1. Download the latest **Flutter SDK** from [flutter.dev](https://docs.flutter.dev/get-started/install/linux#install-flutter-manually)
2. Extract the file in the desired location:
   ```sh
   tar xf ~/Downloads/flutter_linux_3.7.1-stable.tar.xz
   ```
3. Add the `flutter` tool to the `PATH`:
   ```sh
   export PATH="$PATH:`pwd`/flutter/bin"
   ```
4. Run `flutter doctor` to see if there are any dependencies we need to install to complete the setup

## Creating an app
- We can create an app with `flutter create <app-name>`
- Everything we need to develop the app is defined in the `flutter/material.dart` package:
  ```dart
  import "package:flutter/material.dart";
  ```
- The `main` method runs our app by calling `runApp(Widget app)` passing it our custom application widget as a parameter:
  ```dart
  void main() => runApp(MyApp());
  ```

## Widgets
- Everything in **Flutter** is a widget
- Uses **Material Design**
- Every widget has a `build` method that describes the part of the user interface represented by the widget
- Widgets come in two flavors: `StatelessWidget` and `StatefulWidget`
- Many [built-in widgets](#built-in-widgets) already provided (e.g. `Scaffold`, `AppBar`, etc.)

### Stateless widgets
Widgets with immutable state that can **NOT** be changed during runtime:

```dart
import "package:flutter/material.dart";

class StartScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      // Here we can design the UI of this screen
    );
  }
}
```

### Stateful widgets
Widgets with mutable state that can be redrawn on the screen multiple times:

```dart
import "package:flutter/material.dart";

class StartScreen extends StatefulWidget {
  @override
  _StartScreenState createState() => _StartScreenState();
}

class _StartScreenState extends State<StartScreen> {
  @override
  Widget build(BuildContext context) {
    return Container(
      // Here we can design the UI of this stateful screen
    );
  }
}
```

We should call the `setState(VoidCallback fn)` method whenever we want to update our widgets state: 

```dart
setState(() { _myState = newValue; });
```

### Built-in widgets
#### MaterialApp
A convenience widget that wraps a number of widgets that are commonly required for **Material Design** applications.

**Properties:**

- `home` - the widget for the default route of the app
- `theme` - a [ThemeData](#themedata) object for defining an app-wide theme
- `debugShowCheckedModeBanner` - can be used to turn off the little `DEBUG` banner in debug mode

#### Scaffold
Implements the basic **Material Design** visual layout structure.

**Properties:**

- `appBar` - an app bar to display at the top of the `Scaffold`
- `body` - the primary content of the `Scaffold`
- `floatingActionButton` - a button displayed floating above `body`, in the bottom right corner
- `bottomNavigationBar` - a navigation bar to display at the bottom of the `Scaffold`

#### FloatingActionButton
A circular icon button that hovers over content to promote a primary action in the application.

**Properties:**

- `child` - the widget below this button in the tree, typically an [Icon](#icon)
- `onPressed` - the callback that is called when the button is tapped or otherwise activated

#### AppBar
An app bar consists of a toolbar and potentially other widgets, typically exposing one or more common actions with
[IconButton](#iconbutton)s.

**Properties:**

- `title` - the primary widget displayed in the app bar
- `actions` - a list of `Widget`s (`List<Widget>`) to display in a row after the `title` widget
- `leading` - a widget to display before the toolbar's `title`
- `automaticallyImplyLeading` - controls whether the `leading` widget should be implied if `null`

#### NavigationBar
Offers a persistent and convenient way to switch between primary destinations in an app.

**Properties:**

- `destinations` - the list of destinations (`List<NavigationDestination>`)
- `onDestinationSelected` - called when one of the destinations in selected 

#### NavigationDestination
Displays a label below an icon. Used with `NavigationBar.destinations`.

**Properties:**

- `icon` - the icon that's displayed
- `label` - the text label that appears below the icon

#### Navigator
A widget that manages a set of child widgets with a stack discipline.

Typical usage is as follows:

```dart
Navigator.of(context).push(route);
Navigator.of(context).pop();
```

#### MaterialPageRoute
A modal route that replaces the entire screen with a platform-adaptive transition.

**Properties:**

- `builder` - builds the primary contents of the route

#### ThemeData
Defines the configuration of the overall visual theme for a [MaterialApp](#materialapp) or a widget subtree
within the app.

**Properties:**

- `primaryColor` - the background color for major parts of the app (toolbars, tab bars, etc.)

#### Text
A run of text with a single style

**Properties:**

- `style` - the style to use for the text (`TextStyle`)

#### TextStyle
An immutable style describing how to format and paint text.

**Properties:**

- `fontSize` - the size of glyphs (in logical pixels) to use when painting the text
- `color` - the color to use when painting the text

#### ListView
A scrollable list of widgets arranged linearly.

Usually constructed using the `ListView.builder` constructor whose most commonly used parameters include:

- `padding` - the amount of space by which to inset the children
- `itemCount` - the number of items (helps the `ListView` estimate the maximum scroll extent)
- `itemBuilder` - a callback that creates the items in the list

#### ListTile
A single fixed-height row that typically contains some text as well as a leading or trailing icon.

**Properties:**

- `title` - the primary content of the list tile (typically a [Text](#text) widget)
- `leading` - a widget to display before the `title` (typically an [Icon](#icon) widget)
- `trailing` - a widget to display after the `title` (typically an [Icon](#icon) widget)
- `onTap` - called when the user taps the list tile

**Static methods:**
- `divideTiles({ BuildContext? context, Iterable<Widget> tiles })` - adds a one pixel border in between each tile

#### Center
A widget that centers its child within itself.

**Properties:**

- `child` - the widget below the `Center` widget in the tree

#### Row
A widget that displays its children in a horizontal array.

**Properties:**

- `children` - the widgets below the `Row` in the tree
- `mainAxisAlignment` - how the children should be placed along the main axis
- `crossAxisAlignment` - how the children should be placed along the cross axis

#### Column
A widget that displays its children in a vertical array.

**Properties:**

- `children` - the widgets below the `Row` in the tree
- `mainAxisAlignment` - how the children should be placed along the main axis
- `crossAxisAlignment` - how the children should be placed along the cross axis

#### Container
A convenience widget that combines common painting, positioning, and sizing widgets.

**Properties:**

- `child` - the child contained by the container
- `decoration`- the decoration to paint behind the `child`
- `margin` - empty space to surround the `decoration` and `child`
- `padding` - empty space to inscribe inside the `decoration` (the `child`, if any, is placed inside)
- `width` - the width of the `child`
- `height` - the height of the `child`
- `color` - the color to paint behind the `child`

#### Divider
A thin horizontal line, with padding on either side.

**Properties:**

- `color` - the color to use when painting the line

#### Image
A widget that displays an image.

Several constructors are provided for the various ways that an image can be specified:

- `Image.new` - obtain an image from an `ImageProvider`
- `Image.asset` - obtain an image from an `AssetBundle` using a key (defined in `pubspec.yaml`)
- `Image.network` - obtain an image from a `URL`
- `Image.file` - obtain an image from a `File`
- `Image.memory` - obtain an image from a `Uint8List`

#### Icon
A graphical icon widget drawn with a glyph from a font.

**Properties:**

- `icon` - the icon to display

#### IconButton
A picture printed on a **Material** widget that reacts to touches by filling with color.

**Properties:**

- `icon` - the icon to display
- `onPressed` - called when the button is tapped or otherwise activated

#### ElevatedButton
A button whose `elevation` increases when pressed.

**Properties:**

- `child` - typically the button's label
- `style` - customizes the button's appearance
- `onPressed` - called when the button is tapped or otherwise activated

#### OutlinedButton
A medium-emphasis button, containing actions that are important, but not the primary action in an app.

**Properties:**

- `child` - typically the button's label
- `onPressed` - called when the button is tapped or otherwise activated

#### TextButton
A button commonly used on toolbars, in dialogs, or inline with other content.

**Properties:**

- `child` - typically the button's label
- `onPressed` - called when the button is tapped or otherwise activated

#### SizedBox
A box with a specified size.

**Properties:**

- `width` - if non-null, requires the child to have exactly this width
- `height` - if non-null, requires the child to have exactly this height

#### GestureDetector
A widget that detects gestures.

**Properties:**

- `behavior` - how the hit test propagates to children and whether to consider targets behind this one
- `onTap` - triggered when a tap with a primary button has occurred

#### Switch
A widget used to toggle the on/off state of a single setting.

**Properties:**

- `value` - whether this switch is on or off (must not be `null`)
- `onChanged` - called when the user toggles the switch on or off

#### Checkbox
A checkbox that can optionally display three values (tristate).

**Properties:**

- `tristate` - if `true` the checkbox's value can be `true`, `false`, or `null`
- `value` - whether this checkbox is checked
- `onChanged` - called when the value of the checkbox should change

#### SingleChildScrollView
A box in which a single widget can be scrolled.

**Properties:**

- `child` - the widget that scrolls

## Utility classes
### Colors
`Color` and `ColorSwatch` constants which represent **Material Design**'s color palette (e.g. `Colors.green(400)`, etc.).

### Icons
Identifiers for the supported **Material Icons** (e.g. `Icons.favorite`, etc.).

### EdgeInsets
An immutable set of offsets in each of the four cardinal directions.

**Constructors:**

- `all` - creates insets where all the offsets are of the specified `value`
- `only` - creates insets with only the given values non-zero

**Properties:**

- `top` - the offset from the top
- `bottom` - the offset from the bottom
- `left` - the offset from the left
- `right` - the offset from the right
