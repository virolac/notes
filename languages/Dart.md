# Dart

## General
- Statically-typed compiled programming language
- Supports both *Ahead-Of-Time* (**AOT**) and *Just-In-Time* (**JIT**) compilation
- Supports type inference with the `var` keyword
- **Dart** programs end with a `.dart` extension
- Every program starts executing in the `main` function
- The `void` return type can be omitted
- Can run a program with `dart program.dart`
- Everything in **Dart** is an object
- The core functionality is in the `dart:core` package that is automatically imported

## Comments
- Can write in-line comments with `//`
- Can write block comments with `/* */`
- Can write documentation comments with `///`

## Primitive types
- `int` - integer, non-fractional values
- `double` - 64-bit floating point numbers
- `String` - a sequence of **UTF-16** code units
- `bool` - **true** or **false**
- `dynamic` - can implicitly store any value

## Strings
- The `String` data type represents a sequence of characters
- A **Dart** string is a sequence of **UTF-16** code units
- Strings are immutable
- Single line strings are represented using single or double quotes
- Triple quotes are used to represent multi-line strings
- Raw strings can be defined with `r""`
- Can concatenate strings using the `+` operator
- Can interpolate **Dart** expressions within strings using `${}`  
  e.g. `int n = 2; print("The sum of 1 and 1 is ${n};"`
- Can convert a `String` to another type using the `parse(String str)` method (defined by primitive types)
  e.g. `int.parse("3");`
- The `tryParse(String str)` method is similar to `parse` but in case of an error returns `null` instead of throwing an exception
- Can convert a primitive type to a `String` using the `toString()` or `toStringAsFixed(fractionDigits)` method
- String properties:
    - `codeUnits` - returns an unmodifiable list of the **UTF-16** code units of the string
    - `isEmpty` - returns `true` if the string is empty
    - `Length` - returns the length of the string including space, tab and newline characters
- String methods:
    - `toLowerCase()` - converts all characters in the string to lower case
    - `toUpperCase()` - converts all characters in the string to upper case
    - `trim()` - returns the string without any leading and trailing whitespace
    - `compareTo(String str)` - compares this string to `str` and return `-1`, `0` or `1` depending on the relative order
    - `replaceAll(Pattern from, String replace)` - replaces all substrings that match `from` with `replace`
    - `split(Pattern pattern)` - splits the string at matches of `pattern` and returns a list of substrings
    - `substring(int start, [int? end])` - returns the substring of this string from `start`, inclusive, to `end`, exclusive
    - `codeUnitAt(int index)` - returns the 16-bit **UTF-16** code unit at the given `index`

## Operators
### Assignment
- `=` - assigns the right-hand side expression to the variable on the left-hand side
- `op=` - combination of assignment and the binary operator `op`  
  e.g. `x += 2;`

### Unary
- `++` - equivalent to `var = var + 1;`
- `--` - equivalent to `var = var - 1;`

### Ternary
- `condition ? trueValue : falseValue` - evaluates to either `trueValue` or `falseValue` based on the truthiness of `condition`

### Relational
- `==` - equality
- `!=` - inequality
- `>` - greater than
- `>=` - greater than or equal
- `<` - less than
- `<=` - less than or equal

### Logical
- `&&` - logical **AND**
- `||` - logical **OR**

### Null-aware
- `?.` - guards access to a property or method of an object that might be null
- `??` - returns the expression on its left unless that expression's value is `null`, in which case it evaluates and returns the expression on its right
- `??=` - assigns a value to a variable only if that variable is currently `null`

### Type test & conversion
- `is` - returns `true` if an object is of the specified type  
   e.g. `if (employee is Person) {}`
- `as` - casts an object to a particular type  
  e.g. `(employee as Person).firstName = "Bob";`

## Conditionals
### if
```dart
if (number % 2 == 0) {
  print("Even");
} else if (number % 2 == 1) {
  print("Odd");
} else {
  print("Confused");
}
```

### switch
```dart
switch (number % 2) {
  case 0:
    print("Even");
    break;
  case 1:
    print("Odd");
    break;
  default:
    print("Confused");
    break;
}
```

## Loops
### for
```dart
for (var i = 1; i <= 10; ++i) {
  print(i);
}
```

### for-in
```dart
var numbers = [1, 2, 3];

for (var n in numbers) {
  print(n);
}
```

### forEach
```dart
var numbers = [1, 2, 3];

numbers.forEach((n) => print(n));
```

### while
```dart
int num = 5;

while (num > 0) {
  print(num);
  num -= 1;
}
```

### do-while
```dart
int num = 5;

do {
  print(num);
  num -= 1;
} while (num > 0);
```

### break
```dart
int count = 1;

while (count <= 10) {
  count++;

  if (count == 4) {
    break;
  }
}
```

### continue
```dart
int count = 0;

while (count <= 10) {
  count++;

  if (count == 4) {
    print("Number 4 is skipped");
    continue;
  }
}
```

## Collections
### List
An indexable collection of objects with a length.

- Can be non-homogeneous
- Assignment makes a shallow copy
- Can use the spread operator to create a deep copy
  e.g. `var copy = [...original];`

**Example:**
```dart
List<String> names = ["Jack", "Jill"];
print(names[0]);
print(names.length);
```

### Set
A collection of objects in which each object can occur only once.

**Example:**
```dart
var halogens = {"fluorine", "chlorine"};
```

### Map
A collection of key/value pairs.

**Example:**
```dart
var gifts = {
  "first": "partridge",
  "second": "turtledoves",
  "fifth": "golden rings"
};

print(gifts["fifth"]);
```

## Functions
### Basic usage
```dart
bool isNoble(int atomicNumber) {
  return _nobleGases[atomicNumber] != null;
}

isNoble(30);
```

### Anonymous functions
```dart
var list = ["apples", "bananas", "oranges"];

list.forEach((fruit) {
  print(fruit);
});
```

### Arrow functions
```dart
dynamic square(var num) => num * num;
```

### Optional parameters
```dart
dynamic sum(var num1, [var num2 = 0]) => num1 + num2;

print(sum(10));
```

### Named parameters
```dart
// Named parameters are optional by default
dynamic sum({var num1, var num2}) => num1 * num2;

print(sum(num2: 4, num1: 2));
```

## Classes
### Declaring a class
```dart
class class_name {
  <fields>
  <getters/setters>
  <constructors>
  <methods>
}
```

A class definition can include the following:
- **Fields:** A field is any variable declared in a class
- **Setters and Getters:** Special methods that provide read and write access to an object's properties
- **Constructors:** Responsible for allocating memory for and initializing the fields of the class
- **Methods:** Functions associated with the objects of the class

### Constructors
#### Basic syntax
```dart
class Person {
  String name;
  int age;

  Person(String name, [int age = 18]) {
    this.name = name;
    this.age = age;
  }
```

#### Shorthand syntax
```dart
class Person {
  String name;
  int age;

  Person(this.name, [this.age = 18]);
}
```

#### Named constructors
```dart
class Person {
  String name;
  int age;

  Person.guest() {
    name = "Guest";
    age = 18;
  }
}
```

### static, const & final
- `static` - makes a member available on the class itself instead of on the instances of the class
- `const` - a compile-time constant that has to be initialized when declared and can't be changed afterwards
- `final` - a runtime constant that has to be assigned exactly once and can't be changed afterwards

### Inheritance
```dart
class Vehicle {
  String model;
  int year;

  Vehicle(this.model, this.year) {
    print(this.model);
    print(this.year);
  }

  void showOutput() {
    print(model);
    print(year);
  }
}

class Car extends Vehicle {
  double price;

  Car(String model, int year, this.price) : super(model, year);

  @override
  void showOutput() {
    super.showOutput();
    print(this.price);
  }
}
```

### Getters & Setters
```dart
class Rectangle {
  num left, top, width, height;

  Rectangle(this.left, this.top, this.width, this.height);

  // Define two calculated properties: right and bottom
  num get right => left + width;
  set right(num value) => left = value - width;

  num get bottom => top + height;
  set bottom(num value) => top = value - height;
}
```

## Exception handling
```dart
try {
  breedMoreLlamas();
} on OutOfLlamasException {
  // A specific exception
  buyMoreLlamas();
} on Exception catch (e) {
  // Anything else that is an exception
  print("Unknown exception: $e");
} catch (e) {
  // No specified type, handles all
  print("Something really unknown: $e");
} finally {
  // This always runs regardless if an exception was thrown or not
}
```
