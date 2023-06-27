# Kotlin Code Style Guide

This guide outlines the recommended coding style and best practices for writing Kotlin code.

## Table of Contents
1. [General Guidelines](#general-guidelines)
2. [Naming Conventions](#naming-conventions)
3. [Spacing and Indentation](#spacing-and-indentation)
4. [Variables](#variables)
5. [Methods and Functions](#methods-and-functions)
6. [Typealiases](#typealiases)
7. [Conditional Statements and Loops](#conditional-statements-and-loops)
8. [Error Handling](#error-handling)
9. [Comments](#comments)
10. [Testing](#testing)

## General Guidelines

- Use descriptive names for variables, functions, classes, and other identifiers.
- Keep functions short and focused on a single task.
- Avoid deep nesting of code blocks.
- Use proper error handling and avoid swallowing exceptions.
- Write self-explanatory code and avoid unnecessary comments.
- Follow the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles when designing classes and modules.

## Naming Conventions

- Use camelCase for variables, functions, and object properties.
- Use UpperCase for classes and constructor functions.
- Use uppercase snake_case for constants and property names.
- Use underscore before backing property name 

## Class layout

The contents of a class should go in the following order:

- Property declarations and initializer blocks
- Secondary constructors
- Method declarations
- Companion object

Do not sort the method declarations alphabetically or by visibility, and do not separate regular methods from extension methods. 
Instead, put related stuff together, so that someone reading the class from top to bottom can follow the logic of what's happening. 
Choose an order (either higher-level stuff first, or vice versa) and stick to it.

Put nested classes next to the code that uses those classes. If the classes are intended to be used externally and aren't referenced inside the class, put them in the end, after the companion object.

## Interface implementation layout

When implementing an interface, keep the implementing members in the same order as members of the interface (if necessary, interspersed with additional private methods used for the implementation).

## Overload layout

Always put overloads next to each other in a class.

## Spacing and indentation

### Indentation

Use four spaces for indentation. Do not use tabs.

For curly braces, put the opening brace at the end of the line where the construct begins, and the closing brace on a separate line aligned horizontally with the opening construct.

```Kotlin
if (elements != null) {
    for (element in elements) {
        // ...
    }
}
```

### Horizontal whitespaces

- Put spaces around binary operators (a + b). Exception: don't put spaces around the "range to" operator (0..i).

- Do not put spaces around unary operators (a++).

- Put spaces between control flow keywords (if, when, for, and while) and the corresponding opening parenthesis.

- Do not put a space before an opening parenthesis in a primary constructor declaration, method declaration or method call.

```Kotlin
class A(val x: Int)

fun foo(x: Int) { ... }

fun bar() {
    foo(1)
}
```

- Never put a space after (, [, or before ], )

- Never put a space around `.` or `?.` : `foo.bar().filter { it > 2 }.joinToString()`, `foo?.bar()`

- Put a space after `//`: `// This is a comment`

- Do not put spaces around angle brackets used to specify type parameters: class Map<K, V> { ... }

- Do not put spaces around `::` : `Foo::class`, `String::length`

- Do not put a space before `?` used to mark a nullable type: `String?`

As a general rule, avoid horizontal alignment of any kind. Renaming an identifier to a name with a different length should not affect the formatting of either the declaration or any of the usages.

### Colon

Put a space before `:` in the following cases:

 - when it's used to separate a type and a supertype

 - when delegating to a superclass constructor or a different constructor of the same class

- after the object keyword

Don't put a space before `:` when it separates a declaration and its type.

Always put a space after `:`.

```Kotlin
abstract class Foo<out T : Any> : IFoo {
    abstract fun foo(a: Int): T
}

class FooImpl : Foo() {
    constructor(x: String) : this(x) { /*...*/ }

    val x = object : IFoo { /*...*/ }
}
```

### Class headers

Classes with a few primary constructor parameters can be written in a single line:

```Kotlin
class Person(id: Int, name: String)
```
Classes with longer headers should be formatted so that each primary constructor parameter is in a separate line with indentation. Also, the closing parenthesis should be on a new line. If you use inheritance, the superclass constructor call, or the list of implemented interfaces should be located on the same line as the parenthesis:

```Kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name) { /*...*/ }
```
For multiple interfaces, the superclass constructor call should be located first and then each interface should be located in a different line:

```Kotlin
class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name),
    KotlinMaker { /*...*/ }
```
For classes with a long supertype list, put a line break after the colon and align all supertype names horizontally:

```Kotlin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne {

    fun foo() { /*...*/ }
}
```
To clearly separate the class header and body when the class header is long, either put a blank line following the class header (as in the example above), or put the opening curly brace on a separate line:

```Koltin
class MyFavouriteVeryLongClassHolder :
    MyLongHolder<MyFavouriteVeryLongClass>(),
    SomeOtherInterface,
    AndAnotherOne
{
    fun foo() { /*...*/ }
}
```
Use regular indent (four spaces) for constructor parameters. This ensures that properties declared in the primary constructor have the same indentation as properties declared in the body of a class.

## Variables

- Use `val` for variables that will not be reassigned, and `var` for variables that will be reassigned.
- Use `const` only for static variables.
- Declare each variable on a separate line to improve readability.
- Initialize variables at the point of declaration whenever possible.
- Avoid global variables and minimize the scope of variables.
- Use lateinit var only when really need it.

## Methods and functions

- Use meaningful and descriptive method names.
- Limit the number of method's arguments to improve readability.
- Avoid unnecessary side effects in methods.
- Put spaces around the = sign separating the argument name and value.
- If a function returns Unit, the return type should be omitted.
- Omit semicolons whenever possible.

## Type aliases

If you have a functional type or a type with type parameters which is used multiple times in a codebase, prefer defining a type alias for it:

```Kotlin
typealias MouseClickHandler = (Any, MouseEvent) -> Unit
typealias PersonIndex = Map<String, Person>
```

## Conditional Statements and Loops

- Use structural equality (`==` and `!=`) and referential equality(`===` and `!==`)for comparisons.
- Avoid unnecessary negations (`!`) in conditions.
- Use `for` loops for iterating over arrays and other iterable objects.
- Prefer `forEach` over `for` for iterating over object keys.
- Use `until` for iteration over array and other iteratable objects in reverse order.

## Error Handling

- Use `try...catch` blocks for catching and handling exceptions.
- Avoid catching generic `Error` objects unless necessary.
- Always provide informative error messages when throwing exceptions.

## Comments

- Use comments to explain complex algorithms, business rules, or any non-obvious code.
- Write self-explanatory code and avoid excessive comments.
- Remove commented-out code before committing changes.

## Testing

- Write unit tests for critical and complex functionality.
- Use a testing framework such as [JUnit](https://junit.org/junit5/).
- Separate test files from source files and follow a consistent naming convention.
- Run tests automatically as part of the development process.
