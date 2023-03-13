# TypeScript

## General
- Not a new programming language; it's just **JavaScript** with added type safety
- Weird behaviors in **JavaScript** that **TypeScript** can help with:
  ```javascript
  2 + '2' = '22'
  null + 2 = 2
  undefined + 2 = NaN
  ```
- Just a developer tool for statically checking **JavaScript** and transpiling to it; the runtime still runs
  **JavaScript**
- Even if the compiler produces errors, we can still transpile to **JavaScript**
- We can install **TypeScript** with `npm install typescript` (or globally by adding the `-g` option)
- We can transpile to **Javascript** with `tsc <name>.ts`
- We can run `tsc --init` to create a `tsconfig.json` file containing configuration properties for the transpiler

## Types
We can declare the type of a variable with `let <variable>: <type>;`.

However, there are several places where **TypeScript** can provide type information when there is no explicit *type
annotation*. This is called **type inference** and it is recommended to omit *type annotations* in those obvious cases.

### Boolean
A `true` or `false` value.

```typescript
let isDone: boolean = false;
```

### Number
A floating point value.

In addition to *hexadecimal* and *decimal* literals, **TypeScript** also supports *binary* and *octal* literals
introduced in **ECMAScript 2015**. It also supports *BigIntegers*.

```typescript
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

### String
A textual datatype.

Just like in **JavaScript**, strings use either single quotes (`'`) or double quotes (`"`).

We can also use *template strings*, which can span multiple lines and have embedded expressions. These strings are
surrounded by the backtick/backquote (```) character, and embedded expressions are of the form `${ expr }`.

```typescript
let color: string = "blue";
color = 'red';
let sentence: string = `Hello, my favorite color is ${color}.`;
```

### Array
An array of values.

Array types can be written in one of two ways:

- Using the type of elements followed by `[]` to denote an array of that element type
  ```typescript
  let list: number[] = [1, 2, 3];
  ```
- Using the generic `Array` type
  ```typescript
  let list: Array<number> = [1, 2, 3];
  ```

### Tuple
An array with a fixed number of elements whose types are known, but need not be the same.

```typescript
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

Accessing an element outside the set of known indices fails with an error:

```typescript
x[3] = "world";
```
<div style="background-color: #fee; padding: 8px;">
  Tuple type '[string, number]' of length '2' has no element at index '3'.
</div>
<br>

Unfortunately, tuples can still be mutated and array methods such as `push` work on them.

### Enum
A set of numeric values given more friendly, symbolic names.

By default enums begin numbering their members starting at `0`. We can change this by manually setting the value of one of
its members or even manually set all the values in the enum.

```typescript
enum Color {
  Red = 1,
  Green,
  Blue
}

let c: Color = Color.Green; // has numeric value `2`
```

A handy feature of enums is that we can also go from a numeric value to the name of that value in the enum:

```typescript
let colorName: string = Color[2];

// Displays 'Green'
console.log(colorName);
```

We can even choose [string](#string) as the value but strings don't follow a pattern like numbers (1, 2, ...) so we will
have to choose a value for all constants following the string in the enum, until we assign a value to a constant that
has a pattern, like a number:

```typescript
enum WeirdEnum {
  NumValue, // has numeric value `0`
  StringValue = "foo",
  AnotherValue = 3, // have to assign a value because there is no obvious value after "foo"
  InferredValue // value is inferred to be `4`
}
```

### Unknown
A value whose type we don't know (e.g. because it came from the user).

```typescript
let notSure: unknown = 4;
notSure = "maybe a string instead";

// OK, definitely a boolean
notSure = false;
```

We can narrow an `unknown` variable to something more specific by doing `typeof` checks:

```typescript
declare const maybe: unknown;

if (typeof maybe === "string") {
  // TypeScript knows that maybe is a string
  const aString: string = maybe;
  // So, it cannot be a boolean
  const aBoolean: boolean = maybe;
}
```
<div style="background-color: #fee; padding: 8px;">
  Type 'string' is not assignable to type 'boolean'.
</div>

### Any
A value we chose to opt-out of type checking for.

```typescript
let looselyType: any = 4;
// OK, ifItExists might exist at runtime
looselyTyped.ifItExists();
// OK, toFixed exists (but the compiler doesn't check)
looselyTyped.toFixed();

let strictlyTyped: unknown = 4;
strictlyTyped.toFixed();
```
<div style="background-color: #fee; padding: 8px;">
  Object is of type 'unknown'.
</div>
<br>

*Type safety* is one of the main reasons for using **TypeScript** so we should try to avoid using `any` when not
necessary.

Turning on [**noImplicitAny**](https://www.typescriptlang.org/tsconfig#noImplicitAny) will issue an error whenever **TypeScript** would have inferred `any`.

### Void
The absence of having any type at all.

We can only assign `null` or `undefined` to variables of this type.

```typescript
function warnUser(): void {
  console.log("This is my warning message");
}
```

### Null & Undefined
The type of the `null` and `undefined` values respectively.

By default, they are subtypes of all other types, meaning we can assign `null` and `undefined` to something like
`number`. However, when using the [**strictNullChecks**](https://www.typescriptlang.org/tsconfig#strictNullChecks) flag,
`null` and `undefined` are only assignable to `unknown`, `any` and their respective types. `undefined` is also assignable
to `void`.

```typescript
// Not much else we can assign to these variables!
let u: undefined = undefined;
let n: null = null;
```

### Never
The type of values that never occur.

For example `never` is the result of a function that always throws an exception or never returns. Variables also acquire
the type `never` when narrowed by any type guards that can never be true.

The `never` type is a subtype of, and assignable to, every type; however, no type is a subtype of, or assignable to,
`never` (except `never` itself). Even `any` isn't assignable to `never`.

```typescript
// Function returning never must not have a reachable end point
function error(message: string): never {
  throw new Error(message);
}
```

### Object
A non-primitive type, i.e. anything that is not `number`, `string`, `boolean`, `bigint`, `symbol`, `null` or
`undefined`.

Generally, we won't need to use this.

```typescript
declare function create(o: object | null): void;

// OK
create({ prop: 0 });
create(null);

create(42);
```
<div style="background-color: #fee; padding: 8px;">
  Argument of type 'number' is not assignable to parameter of type 'object'.
</div>

## Type aliases
We can write [object](#object) and [union](#union) types directly in *type annotations*, but it's common to want to use
the same type more than once and refer to it by a single name.

A **type alias** is exactly that - a *name* for any type:

```typescript
type User = {
  username: string;
  password: string;
};

function updateUser(user: User): User {}
```

## Interfaces
**TypeScript** also supports interfaces which are very similar to [type aliases](#type-aliases):

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  startTrial: () => string;
          // or
  startTrial(): string;
}
```

We can reopen interfaces:

```typescript
interface User {
  githubToken: string;
}
```

Interfaces support inheritance:

```typescript
interface Admin extends User {
  role: "admin" | "ta" | "learner";
}
```

Differences with [type aliases](#type-aliases):

- Syntactic differences
- Unlike an *interface*, a *type alias* can be used for other types such as primitives, unions, and tuples
- Unlike a *type alias*, an *interface* can be defined multiple times, and will be treated as a single interface (with
  members of all declarations being merged)

## Optional fields
Object types can specify that some or all of their properties are *optional*. To do this, we add a `?` after the
property name:

```typescript
type User = {
  name: string;
  creditCard?: string;
};
```

## Read-only fields
**TypeScript** properties can be marked `readonly` to make them immutable:

```typescript
type User = {
  readonly _id: number;
  name: string;
};
```

## Combining types
### Intersection
Two or more types can be combined with `&` to create a new type that contains the **intersection** of their properties:

```typescript
type CardDetails = CardCompany & CardNumber & { cvv: string };
```

### Union
A **union** type describes a value that can be one of several types:

```typescript
type Score = number | string
```
or even:

```typescript
let seatAllotment: "aisle" | "middle" | "window";
```

## Narrowing tools
- ### typeof
  ```typescript
  typeof padding === "number"
  ```

  <span style="color: red; font-weight: bold;">BEWARE!</span>
  `typeof [] === "object"`

- ### instanceof
  ```typescript
  x instanceof Date
  ```

- ### in
  ```typescript
  "push" in arr
  ```

- ### type assertions
  ```typescript
  document.getElementById("mainCanvas") as HTMLCanvasElement
  ```

  or

  ```typescript
  <HTMLCanvasElement>document.getElementById("mainCanvas")
  ```

- ### type predicates
  ```typescript
  function is Fish(pet: Fish | Bird): pet is Fish {
    return (pet as Fish).swim !== undefined;
  }
  ```

- ### exhaustiveness checking
  ```typescript
  function getArea(shape: Shape) {
    switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.sideLength ** 2;
    default:
      const _exhaustiveCheck: never = shape;
      return _exhaustiveCheck;
    }
  }
  ```

## Functions
- We should always use *type annotations* for function parameters, otherwise they're infered to be `any`
- We can declare default function parameter values:
  ```typescript
  function foo(bar: boolean = true) {}
  ```
- We can (and should) add *type annotations* for function return values:
  ```typescript
  function foo(bar: string): number {}
  ```
- And for arrow functions:
  ```typescript
  (bar: string): number => {}
  ```
- If a function doesn't return a value, we should explicitly add a *type annotation* of `void`
- If a function never returns a value (e.g. it throws an exception or exits the program), we should explicitly add a
  *type annotation* of `never`
- Functions can be declared to accept and/or return objects:
  ```typescript
  function createCourse({ name: string, numStudents: number }): { id: number, isPaid: boolean } {
    // CODE GOES HERE
  }
  ```
- Unfortunately, we can still do this and **TypeScript** won't complain:
  ```typescript
  const courseInfo = { name: "Foo", numStudents: 10, bar: true };

  createCourse(courseInfo)
  ```

## Classes
- We can define classes with the `class` keyword:
  ```typescript
  class User {
    email: string;
    name: string;
    city: string = "";

    constructor(email: string, name: string) {
      this.email = email;
      this.name = name;
    }
  }

  const user = new User("joe@example.com", "Joe");
  user.city = "New York";
  ```
- An alternative way to write the above class that is seen more commonly in production code:
  ```typescript
  class User {
    city: string = "";

    constructor(public email: string, public name: string) {
    }
  }
  ```
- **TypeScript** supports three access modifiers:
  - `public` (default)
  - `private`
  - `protected`
- **TypeScript** supports getters and setters too:
    ```typescript
    class User {
      private courseCount = 1;

      get courseCount(): number {
        return this.courseCount;
      }

      set courseCount(courseNum) {  // NOT ALLOWED TO HAVE A RETURN TYPE ANNOTATION!
        if (courseNum <= 1) {
          throw new Error("Course count should be more than 1");
        }

        this.courseCount = courseNum;
      }
    }
    ```
- Classes can implement interfaces:
  ```typescript
  interface TakePhoto {
    cameraMode: string;
    filter: string;
    burst: number;
  }

  interface Story {
    createStory(): void;
  }

  class Instagram implements TakePhoto, Story {
    constructor(
      public cameraMode: string,
      public filter: string,
      public burst: number,
      public short: string
    ) {}

    createStory(): void {}
  }
  ```
- We can create *abstract* classes and methods:
  ```typescript
  abstract class TakePhoto {
    constructor(
      public cameraMode: string,
      public filter: string
    ) {}

    abstract getSepia(): void;

    getReelTime(): number {
      let reelTime: number;
      // some complex calculation
      //         .
      //         .
      //         .
      return reelTime;
    }
  }

  class Instagram extends TakePhoto {
    constructor(
      public cameraMode: string,
      public filter: string,
      public burst: number
    ) {
      super(cameraMode, filter);
    }

    getSepia(): void {
      // code goes here
    }
  }

  const foo = new Instagram("test", "Test", 3);

  foo.getReelTime();
  ```

## Generics
**TypeScript** supports generics:

```typescript
function identity<T>(val: T): T {
  return val;
}

interface Car {
  brand: string;
  drivetrainType: string;
}

identity(1);
identity("hello");
identity(true);
identity<Car>({ "Mazda", "RWD" });
```

Arrow functions can also be generic[^1]:

```typescript
const foo = <T>(bar: T[]): T => return bar[2];
```

[^1]: *the `<T, >` syntax is used in projects with **JSX** (e.g. **React**) to indicate that `<T>` is syntax for generics,
not a **JSX** tag*

### Generic constraints
#### Interface constraint
```typescript
<T extends Lengthwise>
```
(where `Lengthwise` is an [interface](#interfaces))

#### Object key constraint
```typescript
<T, K extends keyof T>
```

(where `K` has to be a key of `T`)

#### Class constraint
```typescript
<T>(c: { new (): T })
```

or

```typescript
<T>(c: new () => T)
```

## Optional chaining
The **optional chaining** (`?.`) operator accesses an object's property or calls a function. If the object accessed or
the function called using this operator is `undefined` or `null`, the expression short circuits and evaluates to
`undefined` instead of throwing an error:

```typescript
// property access
customer?.birthday

// chaining
customer?.birthday?.getFullYear()

// element access
customers?.[0]

// function call
foo.?()
```
