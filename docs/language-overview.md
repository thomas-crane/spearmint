# Language overview

- [Language overview](#language-overview)
  - [Variables](#variables)
    - [Type annotations](#type-annotations)
    - [Numbers](#numbers)
    - [Strings](#strings)
    - [Booleans](#booleans)
    - [Objects](#objects)
    - [Null (or the lack thereof)](#null-or-the-lack-thereof)
  - [Control flow](#control-flow)
    - [If statements](#if-statements)
    - [For loops](#for-loops)
      - [Ranges](#ranges)
      - [Collections](#collections)
    - [While loops](#while-loops)
      - [Stop and next](#stop-and-next)
  - [Functions](#functions)
    - [Anonymous functions](#anonymous-functions)
  - [When](#when)
  - [Classes](#classes)
    - [Members](#members)
    - [Methods](#methods)
    - [Constructors](#constructors)
    - [Static methods](#static-methods)
    - [Pre assigned members](#pre-assigned-members)
    - [Inheritance](#inheritance)
      - [Open classes](#open-classes)
    - [Abstract classes](#abstract-classes)
      - [Abstract members and methods](#abstract-members-and-methods)
  - [Interfaces](#interfaces)
  - [Enums](#enums)

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

As we have already seen, the `num` type is one of the primitive types in Spearmint. There is no distinction between integers and floating point numbers, every number simply takes on the `num` type.

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

### Null (or the lack thereof)

Spearmint does *not* have a `null` or `undefined` data type. To represent values that may not exist, Spearmint uses the `Maybe` type. This type will be discussed later, but it is important to keep in mind that all variables in Spearmint *must* have a value.

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

### For loops

For loops are declared using the "for ... in" style. For loops can be used to iterate over a collection of items such as an array, or through a range of numbers.

#### Ranges

Ranges are one of the objects in the Spearmint standard library. Because they are so commonly used, there is syntactic sugar to create ranges.

If a range is declared using two dots, the range will be exclusive of the upper bound. That is, a range such as

```sm
let range = 0..10;

```

will include all of the numbers from 0 up to, but not including 10.

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

#### Stop and next

Sometimes it is necessary to skip an iteration of a loop, or break out of the loop completely. In these cases, the `stop` and `next` keywords can be used

The `stop` keyword can be used to break out of the current loop. It can be used in both `while` and `for` statements

```sm
let! i = 0;

while true {
  i += 1;
  if i == 10 {
    stop;
  }
}

// i is now 10.

```

`next` can be used to continue the loop, but stop execution of the current iteration.

```sm
for x in 0..1000 {
  if x % 2 != 0 {
    next;
  }
  print('${x} is even');
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
fn apply(func: (arg: num) => None, value: num) {
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

To construct an instance of a class, Spearmint uses a syntax similar to JSON. Each member of the class must be given a value when the class is constructed.

```sm
let p = Point {
  x: 10,
  y: 20,
  id: 0,
};

```

If a variable has the same name as one of the properties of the class, it can be used without specifying the name of the field.

```sm
let x = 10;
let y = 20;

let p = Point {
  x, y,
  id: 0,
};

```

Instances of classes can always be created like this. There is no way to make the constructor of a class private.

### Static methods

Methods can be declared as static by adding the `static` keyword to the beginning of the declaration.

```sm
class Point {
  // ...

  static new(x: num, y: num) {
    return Point {
      x, y,
      id: 0,
    };
  }
}

let p = Point.new(10, 20);

```

Static methods do not have a `this` variable in their scope and thus cannot access any instance members of the class they belong to.

### Pre assigned members

In some cases it can be desirable to always assign the same value to a class member when that class is constructed. To do this, we can simply assign a value to it when it is declared.

```sm
class MyClass {
  x: num;
  y: num;
  private id = 0;
}

```

If a member is pre assigned, it does not have to be included when constructing an instance of the class. For example,

```sm
// this is fine because `id` is pre assigned
let p = Point {
  x: 10,
  y: 20,
};

// this is still valid and will override the pre assigned value
let p2 = Point {
  x: 20,
  y: 10,
  id: 1,
};

```

Note that `this` cannot be used in pre assigned values.

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

Subclasses inherit all of the fields from their parent class, so the constructor for the subclass will include those fields. For example, given the following structure:

```sm
open class A {
  a: num;
  b: num;
}

class B {
  c: num;
}

```

An instance of `B` would be constructed like this

```sm
let b = B {
  a: 0,
  b: 1,
  c: 2,
};

```

### Abstract classes

Abstract classes are designed to be inherited from, and cannot be instantiated. A class can be marked as abstract using the `abstract` keyword

```sm
abstract class Node {

}

```

Abstract classes are `open` by default. There is no way to create a closed abstract class.

#### Abstract members and methods

Abstract classes can contain members and methods in the same way that normal classes can.

```sm
abstract class Node {
  kind: NodeKind;

  isKind(kind: NodeKind) {
    return this.kind === kind;
  }
}

```

However, methods in abstract classes can be marked as `abstract`. If they are marked as `abstract`, they do not require an implementation.

```sm
abstract class Node {
  kind: NodeKind;

  abstract isKind(kind: NodeKind): bool;
}

```

Because the return type of an abstract method can never be inferred, a type annotation will always be required.

## Interfaces

TODO

## Enums

TODO


