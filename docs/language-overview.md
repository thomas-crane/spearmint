# Language overview

- [Language overview](#language-overview)
  - [Variables](#variables)
    - [Type annotations](#type-annotations)
    - [Numbers](#numbers)
    - [Strings](#strings)
    - [Booleans](#booleans)
    - [Objects](#objects)
  - [Control flow](#control-flow)
    - [If statements](#if-statements)
      - [Logical and equality operators](#logical-and-equality-operators)
    - [For loops](#for-loops)
      - [Ranges](#ranges)
      - [Collections](#collections)
    - [While loops](#while-loops)
  - [Functions](#functions)
    - [Anonymous functions](#anonymous-functions)
  - [When](#when)
  - [Classes](#classes)
    - [Members](#members)
    - [Methods](#methods)
    - [Constructors](#constructors)
      - [Auto generated constructors](#auto-generated-constructors)
      - [Calling the constructor](#calling-the-constructor)
    - [Inheritance](#inheritance)
      - [Open classes](#open-classes)
    - [Abstract classes](#abstract-classes)
  - [Interfaces](#interfaces)

## Variables

Variables in Spearmint are immutable by default and are declared using `let`.

```sm
let x = 10;
let y = 20;
let z = x + y;

```

Variables can be made mutable by using the `let!` keyword instead.

```sm
let! x = 10;
x = x + 20;

```

### Type annotations

Variables can be annotated in order to tell the compiler what types the variable is allowed to be. Type annotations come after the variable name and before the assignment.

```sm
let x: num = 10;

```

In most cases, the compiler can infer the type of the variable from its usage. However, there may be some cases where an explicit type is required.

### Numbers

As we have already seen, the `num` type is one of the primitive types in Spearmint. There is no distinction between integers and floating point numbers, every number simply takes on the `num` type which encapsulates all numbers.

### Strings

The `str` type is another primitive type and is used to represent a string of characters.

```sm
let greeting = 'Hello, World!';

```

### Booleans

The `bool` type is used to represent values that are either true or false.

```sm
let value = true;

```

### Objects

The last primitive type in Spearmint is the object. Objects can be thought of as instances of classes, which will be discussed later. The important thing to remember is that if a variable is not one of the 3 aforementioned primitive types, then it *must* be an object. This has some implications that are discussed later.

## Control flow

### If statements

If statements do not require parentheses around the condition

```sm
if x > 10 {
  // do something
}

```

The body of an if statement *must* be a block statement. Code such as

```sm
if x > 10
  doSomething();

```

is not valid. This is the case for all other control flow statements as well.

#### Logical and equality operators

One notable difference between Spearmint and other C style languages is that Spearmint uses keywords for logical and equality operators. For example,

```sm
if something or otherThing {
  if this is that and foo is! bar {
    // do something
  }
}

```

would be the Spearmint equivalent of the following code in other languages

```txt
if something || otherThing {
  if this == that && foo != bar {

  }
}
```

### For loops

For loops are declared using the "for ... in" style. For loops can be used to iterate over a collection of items such as an array, or through a range of numbers.

#### Ranges

Ranges are one of the built in types of Spearmint. Because they are commonly used in for loops, syntactic sugar can be used to declare a new range.

If a range is declared using two dots, the range will be exclusive of the upper bound. That is, a range such as

```sm
let range = 0..10;

```

will include all of the numbers up to, but not including 10.

An inclusive range can be declared using three dots, like so

```sm
let range = 0...10;

```

A variable that contains a range can be used in a for loop, or the range can just be declared inline.

```sm
// these two loops are equivalent.

let range = 0..10;
for x in range {

}

for x in 0..10 {

}

```

#### Collections

The same syntax can be used to iterate over the elements in a collection

```sm
let items = [1, 2, 3, 4, 5];

for item in items {

}

```

### While loops

Like if statements, while loops do not require parentheses around the condition

```sm
let! x = 0;
while x < 10 {
  x += 1;
}

```

## Functions

Functions in Spearmint are declared using the `fn` keyword.

```sm
fn add(a: num, b: num): num {
  return a + b;
}

```

Several other properties of functions are evident from this code snippet:

+ Arguments are annotated with their type in the form `name: type`
+ Functions are annotated with their return type in a similar way to arguments.
+ The `return` keyword can be used to return a value from the function.

### Anonymous functions

Anonymous functions are declared using "fat arrow" syntax.

```sm
let add = (a: num, b: num) => {
  return a + b;
};
```

Unlike regular functions, anonymous functions do not need to have a block statement as their body. An anonymous function can simply use an expression after the arrow which then becomes the return value of the function.

```sm
let add = (a: num, b: num) => a + b;

```

An important difference between regular functions and anonymous functions is that regular functions are hoisted. This means that while the following code will work

```sm
let x = add(1, 2);

fn add(a: num, b: num) {
  return a + b;
}

```

The same approach cannot be taken when using anonymous functions

```sm
let x = add(1, 2); // error: `add` is undefined

let add = (a: num, b: num) => a + b;

```

Both regular functions and anonymous functions are first class objects in Spearmint, so they can be assigned to variables, passed as parameters and returned from functions just like other data types.

```sm
fn apply(func: (arg: num) => nil, value: num) {
  func(value);
}

```

## When

The when expression is similar to the `match` statement from Rust. It provides a mechanism for executing one of many branches of code depending on the value of something. Like Rust, the cases of the when expression must be exhaustive.

```sm
let x = true;

when x {
  true => print('x is true'),
  false => print('x is false'),
}

```

For types where it would be impossible to manually cover all cases (such as numbers), a named branch can be used to cover any remaining cases.

```sm
let x = 10;

when x {
  1 => print('x is 1'),
  2 => print('x is 2'),
  other => print('x is something else (${other})'),
}

```

Because when expressions are exhaustive, they can be used as an expression in cases where all of the branches return a value.

```sm
let x = 10;

let y = when x {
  10 => 10,
  other => {
    let z = other * 10;
    return z + 3;
  }
}

```

If the when expression has been used where a value is expected (such as in a variable assignment), but some of the branches do not return values, errors will be raised for those branches.

## Classes

Classes are declared using the `class` keyword followed by the name of the class.

```sm
class Point {

}

```

### Members

Members can be added to a class by declaring the name of the member, followed by its type.

```sm
class Point {
  x: num;
  y: num;
}

```

By default, class members are public and immutable. Class members can be made private by using the `private` keyword.

```sm
class Point {
  x: num;
  y: num;

  private id: num;
}
```

Members can be made mutable by using the `mut` keyword.

```sm
class Point {
  mut x: num;
  mut y: num;

  private id: num;
}

```

### Methods

Methods can be added to a class by declaring a function, but without the `fn` keyword.

```sm
class Point {
  // ...

  scale(factor: num) {
    this.x *= factor;
    this.y *= factor;
  }
}
```

Again, the default is that methods are public, but they can be hidden by using `private`.

### Constructors

The constructor function of a class can be defined by adding a method to the class with the name `new`. The constructor of a class can never be private.

```sm
class Point {
  // ...

  new(x: num, y: num) {
    this.x = x;
    this.y = y;
  }
}

```

#### Auto generated constructors

The pattern of declaring a constructor which simply takes the class' members as parameters and assigns the parameters to the members is very common. Because of this, all classes in Spearmint have an automatically generated constructor method which does exactly this.

The auto generated constructor will use the class members as parameters in the order they were declared. This means that if we didn't write a constructor for out `Point` class, the auto generated one would have looked like this.

```sm
class Point {
  // ...

  new(x: num y: num, id: num) {
    this.x = x;
    this.y = y;
    this.id = id;
  }
}

```

Because a class can only have one constructor, manually writing one will override the auto generated constructor.

#### Calling the constructor

To construct a new instance of a class, the class name can simply be used like a method.

```sm
let point = Point(10, 20, 0);

```

### Inheritance

Spearmint supports single inheritance. A class can be extended by using the `extends` keyword.

```sm
class Point3D extends Point {

}

```

Although we have extended the class correctly, this code will raise an error, because the `Point` class is not an "open class".

#### Open classes

By default, all classes are "closed" which means they cannot be inherited from. To allow a class to be inherited, it must explicitly be declared as `open`.

```sm
open class Point {
  // ...
}

```

With this change, our `Point3D` class no longer raises an error.

Auto generated constructors in subclasses will first use the members of the parent class, and then the child class. For example in a structure like this

```sm
open class A {
  a: num;
  b: num;
}

class B {
  c: num;
}

```

The auto generated constructor for `B` would be equivalent to

```sm
class B {
  // ...

  new(a: num, b: num, c: num) {
    super(a, b);
    this.c = c;
  }
}

```

### Abstract classes

TODO

## Interfaces

TODO
