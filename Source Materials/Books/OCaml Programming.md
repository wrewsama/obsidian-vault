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

## Modular Programming
- modules: collection of definitions
- module types: "interfaces" for the modules
```ocaml
module type SERVICE = sig
    val bar : int -> int
    val baz : int
end
module Service : SERVICE = struct
    let bar x = x + 1
    let baz = 35
end
```
- in `utop`, we can `#load` `.cmo` (compiled module object) files to access compiled modules
- abstract types can be parameterised
```ocaml
module type Foo = sig
    type 'a bar
    val baz : 'a bar -> 'a bar 
end
module FooImpl : Foo = struct
    type 'a bar = 'a int
    let baz x = x + 1
end
```
- compilation units
    - pair including 1 `.ml` file and 1 `.mli file`
    - `.ml` file is essentially a module
    - `.mli` file is essentially a module type
- module type constraints (setting things inside the module type): `MyType with type t = int`
- use the `include` keyword to let a module or module type extend from another module / module type
- functors
    - syntax: `module Functor (InputModule : InputModuleType) = ... end`
    - anonymous definition: `functor (InputModule : InputModuleType) -> ...`

## Mutability
- refs
    - a _reference_ to a value in memory (basically a pointer)
    - instantiation: `let x = ref 35` 
    - dereference: `!x`
    - assignment: `x := 67`
    - structural equality checks the underlying value, not the location. i.e. `(ref 35) = (ref 35)` is `true`
- semicolon recap
    - `e1; e2; ... en` evaluates all the expressions from left to right, discarding all the values before `en`, then returns `en` 
    - compiler will display a warning if any of the discarded values are not the unit value `()`
- mutable fields in records
    - `type person = { name : string; mutable age : int }`
    - assignment: `unc.age <- 67`
    - no need any special deference syntax, can read with `unc.age`
- arrays
    - definition: `let arr = [|e1;e2;...en|]`
    - access and modification: `arr.(3) <- 5`
- loops
    - `while condition do e done`
    - `for x=start_int to end_int do e done`
    - `for x=start_int downto end_int do e done`
## Concurrency
- 2 responsibilities of promises: handle client reads (the _promise_), handle library writes (the _resolver_)
- use the `Lwt_io` module from the `lwt.unix` package to get async I/O functions
    - `#require "lwt.unix"`
    - `Lwt_io.read_line`
- callbacks
    - use `lwt.bind` to bind a promise to a callback
        - the callback should take in the content of the promise and return another promise
        - i.e. the promise is a _monad_
    - alternative syntax for `bind promise (fun x -> result)`
        - `promise >>= (fun x -> result)`
        - `let%lwt x = prommise in result`
- monads
    - interface
        - `return x`: the "trivial effect", essentially puts `x` in the monad
        - `bind m f` / `m >>= f`: applies the function `f` to the content of the monad, returning another monad
            - `f`  takes in the monad content and returns a monad
            - `bind` will return a monad of the same type as `f`'s return value, but not necessarily the same monad
    - laws
        - `return x >>= f` behaves the same as `f x`
        - `m >>= return` behaves the same as `m`
        - `(m >>= f) >>= g` behaves the same as `m >>= (fun x -> f x >>= g)` 
            - conceptually, the 2nd expression is `m >>= (f >>= g)` but type checking breaks from it so an anonymous function is required
## Correctness
- function specification documentation in the interface (similar to javadoc comments in the `.mli` file)
- abstraction function: function that maps the concrete value of a type to the abstract value that the client sees
- representation invariant: rules that always apply to the abstraction
- Qcheck
    - property testing with pseudorandom inputs
    - the `Qcheck.int` is called an _arbitrary_, indicating the type of variable to use for the random inputs
```ocaml
let t = Qcheck.Test.make ~count:1000 Qcheck.int property_fun
Qcheck_runner.run_tests [t];;
```
- correctness proofs
    - basic patterns: equality, induction
    - can prove correctness of an algorithm by using a simpler implementation and proving equality (e.g. by induction)
    - termination proof: prove there's some binary relation to order the inputs into a sequence that ends up reaching the base case
- equational specification of a data structure
    - operation types
        - generators: creates a canonical form of the data structure
        - manipulators: returns a manipulated form the data structure
        - queries: returns something else, not the data structure
    - start by creating equations for each (generator, non-generator pair)
---
Source: https://cs3110.github.io/textbook/chapters/preface/about.html
