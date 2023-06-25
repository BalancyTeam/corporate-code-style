# TypeScript Code Style Guide

This guide outlines the recommended coding style and best practices for writing TypeScript code.

## Table of Contents
1. [General Guidelines](#general-guidelines)
2. [Naming Conventions](#naming-conventions)
3. [Spacing and Indentation](#spacing-and-indentation)
4. [Type Annotations](#type-annotations)
5. [Imports](#imports)
6. [Function and Method Definitions](#function-and-method-definitions)
7. [Conditional Statements and Loops](#conditional-statements-and-loops)
8. [Error Handling](#error-handling)
9. [Comments](#comments)
10. [Testing](#testing)

## General Guidelines

- Use descriptive names for variables, functions, classes, and other identifiers.
- Keep functions and methods short and focused on a single task.
- Avoid deep nesting of code blocks.
- Use proper error handling and avoid swallowing exceptions.
- Write self-explanatory code and avoid unnecessary comments.
- Follow the [SOLID](https://en.wikipedia.org/wiki/SOLID) principles when designing classes and modules.

## Naming Conventions

- Use camelCase for variables, functions, and class methods.
- Use PascalCase for classes and interfaces.
- Use uppercase snake_case for constants and enum values.

## Spacing and Indentation

- Use 2 spaces for indentation.
- Use spaces around operators and after commas.
- Place an opening brace on the same line as the statement or declaration.
- Use a space between the `if` keyword and the opening parenthesis.

## Type Annotations

- Prefer explicit type annotations over type inference for variables and function return types.
- Use type inference when the type is clear from the context.
- Use union types and intersection types when appropriate.
- Avoid using the `any` type whenever possible.

## Imports

- Sort imports alphabetically within groups.
- Separate third-party imports from local imports with a blank line.
- Use named imports instead of importing the entire module.
- Avoid importing from index files directly.

## Function and Method Definitions

- Use arrow function syntax (`() =>`) for callbacks and anonymous functions.
- Use function declarations for named functions.
- Use concise method syntax for object methods.
- Use destructuring for function arguments when possible.

## Conditional Statements and Loops

- Use strict equality (`===` and `!==`) for comparisons.
- Avoid unnecessary negations (`!`) in conditions.
- Use `for...of` loops for iterating over arrays and other iterable objects.
- Prefer `for...of` over `for...in` for iterating over object keys.
- Use `Array#forEach` or higher-order array methods when appropriate.

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
- Use a testing framework such as [Jest](https://jestjs.io/) or [Mocha](https://mochajs.org/).
- Separate test files from source files and follow a consistent naming convention.
- Run tests automatically as part of the development process.
