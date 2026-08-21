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
    - pipelining: `5 |> inc |> square` applies inc, then square to 5
- printing
    - `print_endline`
    - `Printf.printf`

## Data and Types
- lists
    - nil (empty list): `[]`
    - cons operator (appends element on the left to list on the right) `x :: lst`
    - syntactic sugar: `[x;y;z]`
    - pattern matching
    ```ocaml
    match lst with
    | [] -> 0
    | h :: t -> h*2 :: t
    ```
- variants: similar to enums, `type oshi = Miko | Suisei | Bijou`
    - can also `match` each key
    - can carry data e.g. `type foo of int * string` carries a tuple with an int and a string
        - the carried data can be parameterised and recursive too
        - e.g. `type 'a tree = Leaf | Node of 'a * 'a tree * 'a tree`
- `OUnit`: unit testing library
- records: similar to structs `type foo = { bar: int; baz: int; }`
    - access with dot notation `foo.bar` or with `match`
- tuples: same as python
- options: similar to the Maybe type with `None`and `Some x`. Can be handled with `match` just like variants
- maps: `[("key1", "value1); ("key2", "value2")]` actually just a list of tuples (not the most efficient kind of map)
- exceptions
    - declaration format: `exception E of t` where `E` is the constructor name and `t` is the type of the data the exception carries. Providing `t` is not necessary
    - raise with `raise`
    - handle with `try e with pattern1 -> foo` (same as the `match` clause)

## Higher-Order Functions
- map: `List.map (fun x -> x*2) lst`
- filter: `List.map (fun x -> (x mod 2 = 0)) lst`
- fold (reduce)
    - fold right: `List.fold_right f lst acc`
        - `f` needs to take in `element, acc`
        - `f` gets applied right-to-left (e.g. folding `[1;2]` applies the function on 2 and init, then 1 with the result of that)
    - fold left
        - tail recursive version of fold right, applies `f` from left to right
        - `f` takes in `acc, element` (opposite of `fold_right`)
        - `List.fold_left f acc lst` (note the signature is difference from `fold_right`)

---
Source: https://cs3110.github.io/textbook/chapters/preface/about.html
