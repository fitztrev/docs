# Conventions

## Naming Conventions

- provide global guidance here for naming functions and variables where possible, customize for each language in specific sections below
- truly general stuff can go here like "prefix boolean methods with is/has/can"

## Scala

- camelCase, first letter capitals for constants, classes, objects, types, blah
- prefer scala3 style (indent unless braces are needed, no-paren ifs, etc)
- prefer extension methods in lila.common.base.\*, ornicar.scalalib, and cats (.some, Option.has, etc)

## Typescript

- follow https://google.github.io/styleguide/tsguide.html except for:
  - feel free to use `#privateIdent` if it allows better minification and compatibility
  - do use `_` prefix for unused arguments
  - no `snake_case` for filenames. Use `camelCase` instead.
  - _jsdoc_ comments are not required, instead you should strive for function, parameter, names, and types so that the LSP info your editor gives is enough.
- random lila bits
- suggest when to bundle module code (common, chess, etc) vs lazy load it (analyse, etc)
- discourage index.d.ts, specify criteria for putting stuff there (solves module dependency, etc)
- index.d.ts not currently watched by ui/build
- interface members
  - use arrow function syntax to define callback signatures
  - traditional function syntax to describe service implementations
- find and link to straightforward explanation of `this` behavior (arrow functions, etc.)

## mongo

- snake_case collections, camelCase fields
