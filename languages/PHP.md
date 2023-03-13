# PHP

## General
- Can render plain text or html markup
- **PHP** code goes between `<?php ?>` tags
- The ending tag is optional if no html markup follows
- Single-line comments: `//`
- Multi-line comments: `/* */`
- Every line must end with a semicolon

## Output
- **echo:** Output string, numbers, html, etc.  
  e.g. `echo 123,  'Hello', true;`
- **print:** Works like echo, but can only take in a single argument  
  e.g. `print 123;`
- **print_r:** Prints single values and arrays  
  e.g. `print_r([1, 2, 3]);`
- **var_dump:** Returns more info, like data type and length  
  e.g. `var_dump('hello') → string(5) 'Hello'`
- **var_export:** Similar to `var_dump`. Outputs a string representation of a variable  
  e.g. `var_export('Hello'); → 'Hello'`
- `<?= 'Hello'; ?>` is shorthand for `<?php echo 'Hello'; ?>`
- Can use the `printf($format, ...$values)` function for formatted output

## Data types
- **null:** Empty variable
- **bool:** true or false
- **int:** Whole number
- **float:** Decimal number
- **string:** Series of characters surrounded by quotes
- **array:** Special variable, which can hold more than one value
- **object:** Created from a class
- **callable:** Callback function
- **resource:** Special variable that holds a resource

## Variables
- Must be prefixed with `$`
- Must start with a letter or the underscore character
- Must not start with a number
- Can only contain alphanumeric characters and underscores (A-z, 0-9 and _)
- Are case-sensitive (`$name`  and `$NAME` are two different variables)
- `snake_case` is the style used most frequently
- Can do arithmetic with numbers as strings  
  e.g. `$x = '5' + '5'; var_dump($x) → int(10)`
- Common arithmetic operators: `+ - * / %`
- Can create compile-time constants with `const`  
  e.g. `const HOST = 'localhost';`
- Can create runtime constants with `define`  
  e.g. `define('HOST',  'localhost');`
- Can check if a variable is set with the `isset($var)` function

## Arrays
- Can create with literal syntax  
  e.g. `$numbers = [1, 44, 55, 22];`
- Can create with the `array(...$items)`  function  
  e.g. `$fruits = array('apple', 'orange', 'pear');`
- Use 0-indexing
- Can create associative arrays  
  e.g. `$hex = ['red' => '#f00', 'green' => '#0f0', 'blue' => '#00f'];`
- Can create multi-dimensional arrays  
  e.g.
  ```php
  $people = [['first_name' => 'Brad', 'last_name' => 'Traversy']];
  echo $people[1]['first_name'];
  ```
- Can convert to and from **JSON** with `json_encode` and `json_decode`
- Can check if an array is empty using the `$empty($array)`  function
- Can get the number of elements with the `count($array)` function
- Can check if an array contains an item with the `in_array($item, $array)` function
- Can add an item to the end of an array:
  - with the `$array[] = $item;` syntax (only one item can be added)
  - with the `array_push($array, ...$items)` function (multiple items can be added)
- Can add an item to the beginning of an array with the `array_unshift($array, $item)` function
- Can remove an item from the end of an array with the `array_pop($array)` function
- Can remove an item from the beginning of an array with the `array_shift($array)` function
- Can remove a specific item with the `unset($item)` function
- Can split an array to chunks of specific size with the `array_chunk($array, $size)` function
- Can merge two arrays together with the `array_merge($array1, $array2)` function
- Can also use the spread operator similar to **JavaScript**  
  e.g. `$arr1 = [1, 2, 3]; [...$arr1, 4] → [1, 2, 3, 4]`
- Can create an associative array from an array of keys and an array of values with the `array_combine($keys, $values)` function
- Can get the keys of an associative array with the `array_keys($array)` function
- Can flip the keys and values of an associative array with the `array_flip($array)` function
- Can create an array from a range of values with the `range($start, $end, $step)` function
- Can create a new array holding the result of calling a function on every element of another array by using the `array_map($callback, $array)` function
- Can get an array with the elements of another array that pass some predicate by using the `array_filter($array, $predicate)` function
- Can reduce the elements of an array to a single value using the `array_reduce($array, $callback, $initial)` function~ 
  e.g. `$sum = array_reduce($numbers, fn($partial_sum, $number) => $partial_sum + $number);`
- Can get the value of the last element in the array with the `end($array)` function

## Strings
- Can concatenate strings with the `.` operator
- Double quotes interpolate variables; single quotes don't  
  e.g. `echo "$name";` or `echo "${name}";`  
- Can get the length of a string with the `strlen($string)` function
- Can find the position of the first occurrence of a substring in a string with the `strpos($string, $substr)` function
- Can find the position of the last occurrence of a substring in a string with the `strrpos($string, $substr)` function
- Can reverse a string with the `strrev($string)` function
- Can convert a string to lowercase with the `strtolower($string)` function and to uppercase with the `strtoupper($string)` function
- Can uppercase the first character of each word with the `ucwords($string)` function
- Can replace a word in a string with another word using the `str_replace($search, $replace, $subject, $count)` function
- Can return a portion of a string with the `substr($string, $offset, $length)` function
- Can check if a string starts or ends with a substring using the `str_starts_with($string, $substr)` or `str_ends_with($string, $substr)` function respectively
- Can split a string to an array of words with the `explode($separator, $string, $limit)` function

## Conditional operators
- **<:** Less than
- **>:** Greater than
- **<=:** Less than or equal to
- **>=:** Greater than or equal to
- **==:** Equal to
- **!=:** Not equal to
- **===:** Identical to
- **!==:** Not identical to

## If statement
```php
$t = date("H");

if ($t < 12) {
    echo 'Good Morning';
} else if ($t < 17) {
    echo 'Good Afternoon';
} else {
    echo 'Good Evening';
}
```

## Ternary operator
```php
$firstPost = !empty($posts) ? $posts[0] : 'No Posts';
```

*It is possible to leave out the middle part of the ternary operator. Expression `expr1 ?: expr3` evaluates to the result of `expr1` if `expr1` evaluates to `true`, and
`expr3` otherwise. `expr1` is only evaluated once in this case.*


## Null coalescing operator
```php
$username = $_GET['user'] ?? 'nobody';
```

## Switch statement

```php
switch ($color) {
    case 'red':
        echo 'Your favorite color is red';
        break;
    case 'blue':
        echo 'Your favorite color is blue';
        break;
    case 'green':
        echo 'Your favorite color is green';
        break;
    default:
        echo 'Your favorite color is not red, green or blue';
}
```

## Loops
### for
```php
for ($x = 0; $x <= 10; $x++) {
    echo 'Number ' . $x . '<br>';
}
```

### while
```php
$x = 0;
while ($x <= 10) {
    echo 'Number ' . $x . '<br>';
    $x++;
}
```

### do while
```php
$x = 1;
do {
   echo 'Number ' . $x . '<br>';
   $x++;
} while ($x <= 5);
```

### foreach
```php
$posts = ['First Post', 'Second Post', 'Third Post'];
foreach ($posts as $index => $post) {
    echo $index + 1 . ' - ' . $post . '<br>';
}
```

## Functions
### Basic usage
```php
$num_users = 12;

function registerUser() {
    global $num_users;
    $user_id = $num_users + 1;
    echo 'User ' . $user_id . ' registered';
}

registerUser();
```

### Default arguments
```php
function sum($n1, $n2 = 10) {
    return $n1 + $n2;
}

sum(5)  // 15
```

### Anonymous functions
```php
$subtract = function($n1, $n2) {
    return $n1 - $n2;
};
```

### Arrow functions
```php
$multiply = fn($n1, $n1) => $n1 * $n2;
```

## Superglobals
Built-in, special variables that are always available in all scopes.
- **$_GET:** Contains information about variables passed through a **URL** or a form
- **$_POST:** Contains information about variables passed through a form
- **$_COOKIE:** Contains information about variables passed through a cookie
- **$_SESSION:** Contains information about variables passed through a session
- **$_SERVER:** Contains information about the server environment
  - `DOCUMENT_ROOT` - the document root directory under which the current script is executing, as defined in the server's configuration file
  - `SERVER_NAME` - the name of the server host under which the current script is executing
  - `SERVER_PORT` - the port on the server machine being used by the web server for communication
  - `PHP_SELF` - the filename of the currently executing script, relative to the document root (e.g. /foo/bar.php)
  - `REQUEST_URI` - the **URI** which was given in order to access this page; for instance,  '/index.html'
  - `SERVER_SOFTWARE` - server identification string, given in the headers when responding to requests
  - `REMOTE_ADDR` - the **IP** address from which the user is viewing the current page
  - `REMOTE_PORT` - the port being used on the user's machine to communicate with the web server
  - `HTTP_` - various request headers are put here after having the name converted appropriately (e.g. `HTTP_USER_AGENT`, `HTTP_HOST`, etc.)
- **$_ENV:** Contains information about the environment variables
- **$_FILES:** Contains information about files uploaded to the script
- **$_REQUEST:** Contains information about variables passed through the form or **URL**

## Input sanitization
- Can use the `htmlspecialchars($string)` function to safely parse user input
- Alternatively, if more flexibility is desired, the `filter_input($type, $var_name, $filter, $options)` function can be used  
  e.g. `filter_input(INPUT_POST, 'name',  FILTER_SANITIZE_SPECIAL_CHARS);`
- There is also a `filter_var($value, $filter, $options)` function that does a similar job for all values, not just inputs  
  e.g. `filter_var('bob@example.com', FILTER_VALIDATE_EMAIL);`

## Cookies
- Can set a cookie with the `setcookie($name, $value, $expires_or_options, $path, $domain, $secure, $httponly)` function  
  e.g. `setcookie('name', 'brad', time() + 60 * 60 * 24 * 30);`
- Can read a cookie with `$_COOKIE[$name]`
- Can delete a cookie by using the `setcookie` function and specifying an expiration time that is in the past

## Sessions
- Can start a session with the `session_start()` function
- Can get/set session variables using the `$_SESSION` superglobal  
  e.g. `$_SESSION['username'] = 'john123';`
- Can destroy a session with the `session_destroy()` function
- The `session_start` function must be called in every script in order to be able to interact with the session in any way
- Can use the `header($header, $replace, $response_code)` function to send a raw **HTTP** header which can be used to redirect the user to another page  
  e.g. `header('Location: http://www.example.com');`

## File handling
- Can check if a file exists with the `file_exists($filename)` function
- Can open a file with the `fopen($filename, $mode)` function that returns a handle to the file
- Can close a file with the `fclose($stream)` function
- Can read up to length bytes from a file with the `fread($stream, $length)` function
- Can write data to a file with the `fwrite($stream, $data, $length)` function
- Can get the size of a file with the `filesize($filename)` function
- Can use the `PHP_EOL` constant to access the platform-appropriate *end-of-line* character
- Can upload files to the server using a form with `enctype="multipart/form-data"` and access them using the `$_FILES[$upload_name]` array
  - `name` - The original name of the file on the client machine
  - `full_path` - The full path as submitted by the browser
  - `type` - The mime type of the file, if the browser provided this information
  - `tmp_name` - The temporary filename of the file in which the uploaded file was stored on the server
  - `error` - The [error code](https://www.php.net/manual/en/features.file-upload.errors.php) associated with this file upload
  - `size` - The size, in bytes, of the uploaded file
- Can move an uploaded file to a new location with the `move_uploaded_file($from, $to)` function

## Exception handling
- Can throw an exception with `throw new Exception($message);`
- Can use `try/catch` blocks with an optional `finally` block, similar to other languages
```php
function inverse($n) {
    if ($n == 0)
        throw new Exception('Division by zero');
    return 1 / $n;
}

try {
    inverse(0);
} catch (Exception $e) {
    echo 'Caught Exception', $e->getMessage(), ' ';
} finally {
    // do some cleanup
}
```

## OOP
```php
class User {
    // Access modifiers supported: public, protected, private
    private $name;
    private $email;
    private $password;

    // Constructor
    function __construct($name, $email, $password) {
        $this->name = $name;
        $this->email = $email;
        $this->password = $password;
    }

    function get_name() {
        return $this->name;
    }

    function set_name($name) {
        $this->name = $name;
    }

    function get_email() {
        return $this->email;
    }

    function set_email($email) {
        $this->email = $email;
    }

    function set_password($password) {
        $this->password = $password;
    }
}

// Inheritance
class Employee extends User {
    function __construct($name, $email, $password, $title) {
        parent::__construct($name, $email, $password);
        $this->title = $title;
    }

    function get_title() {
        return $this->title;
    }
}

$user = new User('Brad', 'brad@gmail.com', '1234');
$user->set_password('aoeu');

$employee = new Employee('Sara', 'sara@gmail.com', '43434', 'Manager');
```

## Including scripts
- Can include and evaluate a **PHP** file with `include 'path_to_file.php';`
- The `require('path_to_file.php')` expression is identical to `include` except upon failure it will produce  a fatal `E_COMPILE_ERROR` and halt the script
- The `include_once` and `require_once` expressions work exactly like their counterparts except they check if the file has already been included/required, and if so,
  don't include/require it again

## Alternative Syntax For Control Structures
**PHP** offers an alternative syntax for some of its control structures; namely, `if`, `while`, `for`, `foreach`, and `switch`. In each case, the basic form of the
alternate syntax is to change the opening brace to a colon (`:`) and the closing brace to `endif;`, `endwhile;`, `endfor;`, `endforeach;`, or `endswitch;`, respectively.  
For example:
```php
<?php if ($a == 5): ?>
A is equal to 5
<?php endif; ?>
```

## Namespaces
- Can create a namespace with the `namespace` keyword  
  e.g. `namespace foo;`
- Namespaces can be hierarchical using the `\` separator  
  e.g. `namespace foo\bar;`
- Can import members of a namespace with the use keyword  
  e.g. `use My\Full\Classname;`
- The use keyword can also be used for aliasing when importing  
  e.g. `use My\Full\Classname as Another;`
