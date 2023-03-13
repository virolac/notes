# Scala

## Why learn Scala?
- Cutting edge research language
- Accepted in the industry
- Runs on all major platforms: **JVM**, **JS**, **Native** (and thus **iOS**), **Android** and **GraalVM**
- General purpose programming language
- Pure **OO** language that heavily encourages **FP**
- Great market position: *backend*, *data mining*, *machine learning*, *AI* and more
- High compensation
- Concise syntax (up to *10x* reduction of **LOC**)
- Extremely powerful type system
- Statically typed with local type inference
- Lots of fun

## What is Scala?
- Pure Expression Oriented Language
  - everything is an expression
  - every expression computes a value
  - every value has a type
  - every value is an object, even functions
  - no `goto`, no `break`, no `continue`, inferred `return`, inferred semicolons
  - less unnecessary curly braces `{}` (especially in **Scala 3**), less parenthesis `()`
- **Sca**lable **La**nguage
  - regular
  - concise
  - powerful, not "just" pretty

## Coursier
**Coursier** is the **Scala** application and artifact manager. It can install **Scala** applications and setup our
**Scala** development environment. It can also download and cache artifacts from the web.

### Installation
1. curl -fL "https://github.com/coursier/launchers/raw/master/cs-$(uname -m)-pc-linux.gz" | gzip -d > cs
2. chmod +x cs
3. ./cs setup

## Scala Build Tool
`sbt` is the interactive build tool for **Scala**.

**Commands:**
- `compile` - compile the project
- `run` - run the project
- `clean` - clean the project
- `test:compile` - compile the tests
- `test` - compile and run the tests
- `scalafmtAll` - run scalafmt on all project files

Putting a `~` before a command name will run that command every time the code changes (e.g. `~compile`).

## Giter8
**Giter8** is a command line tool to generate files and directories from templates published on **GitHub** or any other
**git** repository. It's implemented in **Scala** and runs through the `sbt` launcher, but it can produce output for
any purpose.

It can be called from `sbt`'s `new` command as follows:

```sh
sbt new scala/scala-seed.g8
```

## Comments
- `//` - single line
- `/* */` - multi-line
- `/** */` - **Javadoc** comments

## Variables
**Scala** has two types of variables:

- `val` - creates an *immutable* variable (like `final` in **Java**)
- `var` - creates a *mutable* variable

There is also `lazy val` which binds a value lazily (only when it's first used).

## Types
Types are declared with a `:` before the type name:

```scala
val n: Int = 5
```

**Available types:**

- `Any` - the root of the **Scala** class hierarchy
- `AnyVal` - the root class of all value types
- `AnyRef` - the root class of all reference types (alias for `java.lang.Object`)
- `Nothing` - no values (type of [Exceptions](#exceptions))
- `Unit` - only one value, written `()`
- `Null` - only one value, `null`
- `Boolean` - `true` or `false`
- `Char` - 16-bit unsigned **Unicode** character
- `Byte` - values in the range **[-128, 127]**
- `Short` - values in the range **[-32768, 32767]**
- `Int` - values in the range **[-2147483648, 2147483647]**
- `Long` - values in the range **[-9223372036854775808, 9223372036854775807]**
- `Float` - values in the range **[-3.4028235E38, 3.4028235E38]**
- `Double` - values in the range **[-1.7976931348623157E308, 1.7976931348623157E308]**

## Type aliases
A type alias can be used to simplify declaration for complex types, such as parameterized types or function types:

```scala
type N = Int
```

## Comparison operators
- `==, !=, <, >, <=, >=`
- `==` and `!=` delegate to `.equals`
- `eq` and `ne` can be used to test for referential equality

## Numeric literals
- Can be hexadecimal (e.g. `0xff`)
- Octal not supported anymore
- Can separate digits with `_` for readability (e.g. `100_000`)
- Integer literals are inferred to be `Int`
- We can use the `L` suffix to make them `Long`
- `Double` is the default for floating-point numbers; we must use the `f` suffix to make them `Float`

## Characters & Strings
- Characters in **Scala** are defined using single quotes (`'`)
- **Unicode** character literals are defined using `\uHHHH`, where `HHHH` is the code point in hexadecimal form
- Strings are defined using double quotes (`"`)
- Triple quotes (`"""`) are used to define *multiline*, *raw* strings

### String interpolation
**Scala** supports multiple forms of string interpolation, which happen at compile-time:

- The `s` Interpolator:
  ```scala
  s"$name is $age years old"
  ```
- The `f` Interpolator for creating formatted, typesafe strings:
  ```scala
  f"$name%s is $height%2.2f meters tall"
  ```
- The `raw` Interpolator for creating raw strings:
  ```scala
  raw"""|hello\n
        |world $MyFavoriteNumber""".stripMargin
  ```

## if Expression
```scala
if (test1) {
  doX()
} else if (test2) {
  doY()
} else {
  doZ()
}
```

## match Expression
```scala
a match {
  case 0 | "" => false
  case _ => true
}
```

## for Comprehensions
```scala
val lst = List(1, 2, 3)

for (x <- lst) {
  println(x)
}

for {x <- lst; if x < 5} yield {
  i * i
}
```

## Functions
- Functions are defined with the `def` keyword
- If a function doesn't take any arguments, we can omit the parentheses when defining them and when calling them
- Functions are not limited to alphanumeric characters
  ```scala
  def **(x: Int): Int = x * x
  ```
- Functions whose names end in `:` are right-associative
- Functions with weird names (e.g. containing spaces, etc.) can be defined and called with the name inside backticks
- Use the `@scala.annotation.tailrec` annotation on tail recursive functions to get compiler errors on non-tail calls
- **Scala** supports higher-order functions
- Arrow functions use the syntax `f: Int => String`
- **Scala** also supports a more concise arrow function syntax:
  ```scala
  List(6, 2, 5, 9, 8).sortWith(_ <= b)
  ```
- If a function has only one argument, then the `()` can be replaced with `{}`
- We can give default values to parameters:
  ```scala
  def log(message: String, level: String = "INFO") = println(s"$level: $message")
  ```
- When calling functions, we can label the arguments with their parameter names, which allows us to pass them in any
  order:
  ```scala
  def printName(first: String, last: String): Unit =
    println(first + " " + last)

  printName(last = "Smith", first = "John")
  ```
- We can create variable-argument functions:
  ```scala
  def sum(nums: Int*): Int = nums.reduce(_ + _)
  ```
- We can partially apply a function by passing `_` as an argument:
  ```scala
  def sum(x: Int, y: Int): Int = x + y

  val f = sum(10, _)

  println(f(5)) // prints 15
  ```
- We can create curried functions:
  ```scala
  def curriedSum(x: Int) = (y: Int) => x + y
                   //or
  def curriedSum(x: Int)(y: Int): Int = x + y
  ```

## Option
- Represents optional values
- If a value exists, it is wrapped in `Some` (e.g. `Some(3)`)
- If a value doesn't exist, it is `None`
- We can retrieve the wrapped value using the `get` method (throws [exception](#exceptions) if `None`)
- The `getOrElse` method returns the option's value if nonempty, otherwise it returns the specified `default` value
- The `isEmpty` method returns `true` if the option is `None`

## Collections
Collections are immutable by default, unless qualified/imported from the `scala.collection.mutable` package.

### Array
**Scala** arrays correspond one-to-one to **Java** arrays but are generic and can be used as **Scala** sequences
(`Seq`).

```scala
val fruits = Array("apple", "orange", "banana") // syntactic sugar for Array.apply(...)
println(fruits(2))
fruits(2) = "melon" // syntactic sugar for fruits.update(2, "melon")
```

### List
- Linear, immutable sequences (linked-lists):
  ```scala
  val names: List[String] = List("Max", "Tom", "John")

  println(names) // prints List("Max", "Tom", "John")
  println(names(1)) // prints "Tom"
  ```
- The **cons** (`::`) operator can prepend an element to an existing `List` and return a new `List` with the result:
  ```scala
  val lst: List[Int] = List(1, 2, 3)

  println(0 :: lst) // prints List(0, 1, 2, 3)
  println(lst) // the original list (lst) is not modified
  ```
- The `head` method returns the first element of a `List`
- The `tail` method returns all but the first element of a `List`
- The `isEmpty` method returns `true` if the `List` contains no elements
- The `reverse` method reverses a `List`
- The `fill(n){e}` method creates a `List` with `n` elements whose value is the result of evaluating `e`:
  ```scala
  List.fill(5)(2) // results in List(2, 2, 2, 2, 2)
  ```

### Set
- An unordered collection of elements that contains no duplicates:
  ```scala
  val names: Set[String] = Set("Max", "Tom", "John", "Max")

  println(names) // prints Set("Max", "Tom", "John") (possibly in a different order)
  ```
- The `apply` method tests if an element is in the `Set`:
  ```scala
  val names: Set[String] = Set("Max", "Tom", "John")

  println(names("Tom")) // prints true
  println(names("Jack")) // prints false
  ```
- The `isEmpty` method returns `true` if the `Set` contains no elements
- We can concatenate two sets using the `++` operator (**union**)
- We can compute the **intersection** of two sets with the `&` operator or the `intersect` method
- We can get the minimum element of a `Set` using the `min` method
- We can get the maximum element of a `Set` using the `max` method

### Map
- A collection of [key/value pairs](#tuples), where each key is unique:
  ```scala
  val map: Map[Int, String] = Map(801 -> "Max", 802 -> "Tom", 803 -> "John")

  println(map) // prints Map(801 -> "Max", 802 -> "Tom", 803 -> "John")
  println(map(802)) // prints "Tom"
  println(map(804)) // throws `NoSuchElementException`
  ```
- The `get` method returns the value for a key as an [Option](#option), `None` if not found
- The `size` method returns the number of elements in the `Map`
- The `keys` method returns the `Set` of keys in the `Map`
- The `values` method returns an `Iterable` of the values in the `Map`
- The `isEmpty` method returns `true` if the `Map` contains no elements
- The `contains` method returns `true` if the specified key is in the `Map`
- We can concatenate two maps using the `++` operator

### Tuple
- A non-homogeneous, fixed-size collection of elements:
  ```scala
  val tuple: Tuple4 = (1, 2, "hello", true)

  println(tuple) // prints (1,2,"hello",true)
  ```
- The maximum number of elements a tuple can have is 22 (`Tuple22`)
- We can access the elements of a tuple using `._1`, `._2`, etc.
- `Tuple2` pairs can be created using the `->` operator:
  ```scala
  1 -> "Tom"
  // is equivalent to
  (1, "Tom")
  ```

### Useful higher-order methods
Many useful higher-order methods are available on most collection types:

- `foreach` - executes a function for every element
- `map` - applies a function to every element and returns the result
- `mapInPlace` - modifies a mutable sequence by applying a function to all elements
- `filter` - selects all elements which satisfy a predicate
- `find` - finds the first element satisfying a predicate, if any (returns an [Option](#option))
- `partition` - returns a pair of all elements that satisfy the given predicate (`_1`) and all those that don't (`_2`)
- `flatten` - removes a level of nesting from a collection of collections
- `flatMap` - equivalent to `map` followed by `flatten`
- `reduceLeft` - applies a binary operator to all elements, going left to right
- `reduceRight` - applies a binary operator to all elements, going right to left
- `foldLeft` - applies a binary operator to all elements and a start value, going left to right
- `foldRight` - applies a binary operator to all elements and a start value, going right to left
- `scanLeft` - produces a collection of cumulative results of applying a binary operator, going left to right
- `scanRight` - produces a collection of cumulative results of applying a binary operator, going right to left

## Packages
Packages are created by declaring one or more `package` names at the top of a **Scala** file.

```scala
package com.example
package playground
```

This is syntactic sugar for:

```scala
package com.example {
  package playground {
  }
}
```

Imports can be anywhere, not only at the top and can work with anything, not just packages:

```scala
object MyStuff {
  val inside = 1337
}

import MyStuff.inside
```

## Classes
- Unlike **Java**, classes can have different names than the file containing them, and one file can contain multiple
  classes
- Classes are defined with the `class` keyword:
  ```scala
  class User
  ```
- Class keyword also defines a type which leaves in a separate namespace than values:
  ```scala
  class User
  type U = User
  ```
- Adding a parameter list after the class name defines a **primary** constructor for the class:
  ```scala
  class User(var name: String, var age: Int)
  ```
- We can define **auxiliary** constructors with the `this` keyword:
  ```scala
  class User(var name: String, var age: Int) {
    def this() {
      this("Tom")
    }

    def this(name) {
      this(name, 32)
    }
  }
  ```
- Defining a member with `var` makes it mutable, while defining it with `val` makes it immutable:
  ```scala
  class User(val name: String, var age: Int)

  var user = new User("Max", 20)
  // user.name = "John" not allowed
  user.age = 30

  println(user.name) // prints "Max"
  println(user.age) // prints 30
  ```
- Members are `public` by default but we can use the `private` or `protected` access modifier, if needed
- When we only need one object from a class we can create a *singleton class* with the `object` keyword:
  ```scala
  object Main {
    def main(args: Array[String]) {
    }
  }
  ```
- We can express an **is-a** relationship between classes using **inheritance**:
  ```scala
  class Polygon {
    def area: Double = 0.0
  }

  class Rectangle(var width: Double, var height: Double) extends Polygon {
    override def area: Double = width * height
  }

  class Triangle(var width: Double, var height: Double) extends Polygon {
    override def area: Double = width * height / 2
  }
  ```
- We can define **abstract** classes and methods:
  ```scala
  abstract class Polygon {
    def area: Double // method is abstract because it has no body
  }
  ```
- We can use the `sealed` modifier on a class to prevent it from being directly inherited

## Traits
- Similar to Java interfaces but they can contain implementations
- At least one method in a `trait` must be `abstract`
- We can derive a `trait` with the `extends` keyword
- **Mixins** are traits which are used to compose a class
- **Mixins** are added to a class using the `with` keyword
- Classes can only have one superclass but many mixins
- The `override` keyword is not necessary when overriding trait methods
- The **App** `trait` can be used to quickly turn objects into executable programs (no explicit `main` method needed)

**Example:**

```scala
trait Shape {
  def color: String
}

abstract class Polygon {
  def area: Double
}

class Rectangle(var width: Double, var height: Double) extends Polygon with Shape {
  override def area: Double = width * height
  override def color: String = "red"
}
```

### Self-types
Self-types are a way to declare that a trait must be mixed into another trait, even though it doesn't directly extend
it. This makes the members of the dependency available without imports:

```scala
trait User {
  def username: String
}

trait Tweeter {
  this: User => // reassign this
  def tweet(tweetText: String) = println(s"$username: $tweetText")
}

class VerifiedTweeter(val username_: String) extends Tweeter with User { // we mixin `User` because `Tweeter` required it
  def username = s"real $username_"
}

val realBeyoncé = new VerifiedTweeter("Beyoncé")
realBeyoncé.tweet("Just spilled my glass of lemonade")  // prints "real Beyoncé: Just spilled my glass of lemonade"
```

## Exceptions
```scala
try 1 / 0
catch {
  case e: ArithmeticException => 0
}
finally println("no worries, it's all good")
```
