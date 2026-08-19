Tags:
- [[OCaml]]
---
## Intro
- `opam` is the package manager for OCaml (basically pip)
- OPAM switches are like python venvs, except they include a compiler together with the dependency packages. Create one with `opam switch create . COMPILER_VERSION`
    - check with `opam switch list`
- characteristics of OCaml
    - functional (stateless)
    - statically typed
    - type-safe
    - garbage-collected

## Basics of OCaml
- `utop` (Universal Toplevel) is similar to a REPL
    - import code with `#use "foo.ml";;`
    - exit with `#quit;;`
    - note: the `;;` is only required in `utop`, not in the code you write in a `.ml` file
- `dune`: build system
    - `dune init project foo`: initialise a project named `foo`
    - `dune exec bin/main.exe`: compile and run the project
    - `dune build foo.exe`: compile the current project to an executable named `foo.exe`, it can then be executed with `dune exec ./foo.exe`
    - `dune clean`: clean up build artifacts
- expressions
    - the primary task of computation in a functional language is to evaluate an **expression** (e.g. `6+9`) to a **value** (e.g. `15`)
    - `=` and `<>` check structural equality while `==` and `!=` check physical equality
    - conditionals are like ternary operators `if condition then "x" else "y"`
    - `let`
        - `let x = 69` binds `x`
        - `let x = 69 in x - 2` is an expression that evaluates to `67`, the `x=69` only applies in the scope after the `in`
- functions
    - `let f x = x + 1`
    - need to explicitly declare recursive functions with `let rec f x = if n = 0 then 1 else n + f (n-1)`
    - use tail recursion to avoid stack overflows
- printing
    - `print_endline`
    - `Printf.printf`
---
Source: https://cs3110.github.io/textbook/chapters/preface/about.html
