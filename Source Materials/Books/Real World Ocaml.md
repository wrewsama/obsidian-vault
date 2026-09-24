Tags:
- [[Ocaml]]
---
# Language Concepts
---
## Guided Tour
 - `Base` vs `Core`
     - Base: lightweight, portable standard library
     - Core: extension of Base with extra useful modules (e.g. data structures, time, etc.), only usable on UNIX-like systems
 - In Base/Core, operators (e.g. `=`, `+`, etc.) are scoped to each module
     - e.g. `=` only works on integers, need to use `String.(str1 = str2)` for string comparison
     - can do `let open Float.O in float1 + float2 = float3`
     - `+.` also works without opening `Float.O`
     - `**` is integer exponentiation (was float exp in the std lib)
     - `**.` is float exponentiation (also available in `Float.O`)
 - Other primitives (variants, tuples, lists) work the same as the ocaml standard library (note: many of the library functions have different signatures e.g. `List.map`)

## Variables and Functions
- no differences from ocaml standard library
- interesting gotchas
    - when you pass a function with labelled arguments as an argument into another function, the order with which you pass in the labelled arguments matters
    - during type inference, OCaml will prefer labels over options
    - when doing partial application on a function with optional and positional arguments, if you pass in one of the positional arguments, OCaml will _erase_ all the optional arguments defined before the first positional argument
        - by _erase_, the argument isn't gone, it just becomes a normal positional argument
        - the exception is when everything is passed in at the same time (so you can pass in the optional argument at the end without it getting erased)

## Lists and Patterns
- pattern matching and the list type itself are the same as the OCaml standard library
- the `List` module's functions have different signatures
    - the list is passed in as the first argument, other arguments are labelled arguments (e.g. map/filter/fold functions are `~f`)
    - there is only one `fold`, and it behaves like fold_left
- polymorphic compares are unavailable by default in Base, but you can access them in the `Base.Poly` module

## Files, Modules, Programs
- same as standard ocaml
- remember to add `base` to the `libraries` part of the `dune` config
- module opening best practices
    - avoid as much as possible
    - if unavoidable, use local opens
    - alternative option: module shortcuts e.g. `let module R = ReallyLongJavaEsqueModuleName in R.foo ()`

## Records
- punning: shorthand for automatically naming variables based on their labels/field names
    - label punning: `let f ~foo ~bar` = `let f ~foo:foo ~bar:bar`
    - field punning: `{ foo; bar }` = `{ foo=foo; bar=bar }`
        - works for both declaration and pattern matching
- functional update: shorthand to copy all key-values from a record to another, with some changed fields
    - `let rec = {old_rec with foo="something else"`
- `[@@deriving fields]`: annotation that creates getters and setters for the given type
    - note: if you use it on multiple records within the same module, ensure we avoid field name collisions
```ocaml
module Logon = struct
    type t = {
        ...
    }[@@deriving fields]
end;;
```

## Variants
- same as standard ocaml
- notes on polymorphic variants
    - just variants without needing to declare a new `type`
    - can increase complexity, weaken type safety, and worsen efficiency

## Error Handling
- Error-Aware return types
    - option (`None` and `Some a`)
    - result (`Ok a` and `Error b`)
- Exceptions
    - fundamentally the same as standard ocaml
    - can use the `[@@deriving sexp]` annotation to improve the printing of exceptions with record types
    - can use `Exn.protect` to set up a `finally` clause to clean up
    - can use `raise_notrace` instead of `raise` to remove backtraces (better performance but less debugging information)
- `Or_error.try_with`: takes in a thunk, executes it, and returns an error-aware return type based on whether the thunk raised an exception

## Imperative Programming
- refs, arrays, loops, and lazy all work the same way as standard ocaml
- IO (stdin, stdout, stderr, files) is handled by modules in `Core`
    - `In_channel`
    - `Out_channel`

## GADTs
- Generalised Algebraic Data Types: extension of variants
```ocaml
type _ gadt =
    | Int : int -> int gadt
    | Bool : bool -> bool gadt
```
- uses `_` instead of a polymorphic type variable like `'a`
- flexibly allows different constructor inputs to return different variant types

## Functors
- purpose
    - dependency injection
    - extension of modules
    - allow for separate instances of stateful modules (so they don't all share the same state)
- sharing constraints: expose information about a concrete type within the module type
    - `<Module_type> with type <type> = <type'>`
    - can be done in the module type definition, or the return type hint of the functor
- destructive substitution
    - `<Module_type> with type <type> := <type'>` (note the walrus operator instead of equal)
    - replace references to `<type>` in the signature with `<type'>`

## First Class Modules
- allows you to use modules like ordinary values (e.g. to be passed into functions)
- creation: `let first_class_module = (module Module : Module_type)`
- unpacking: `module Unpacked_module = (val first_class_module : Module_type)`
- pattern matching: `let f (module Module : Module_type) = stuff with Module...`
- side topic: locally abstract types
    - syntax: `let f (type a) (x : a): a`
    - introduces an abstract type variable (`a` in this case) to the local scope of the function (`f` here)

## Objects
- basic usage of objects
```ocaml
let my_obj = object
    val my_val = 35
    method my_method x = my_val + x
end
my_obj#my_method 111
```
- basic usage of object types
```ocaml
type my_obj_type = < my_method : int -> int >
let widened = (some_obj_with_my_method : my_obj_type)
```
- NOTE: type narrowing is not permitted in ocaml

## Classes
- class definition (just an object with the class keyword): `class my_class arg1 arg2 ... = object ... end;;`
- instantiation: `let my_obj = new my_class`
- inheritance: `class my_subclass = object inherit my_superclass as localname ... end`
- self: `class my_class ... = object(self) ... end`
    - can access other methods using `self#method_name`
- private methods: `method private my_method = ...`
- virtual classes/methods: `class virtual my_class ...`, `method virtual my_method ...`
- initialisers (like the `__init__` method in python): `class my_class = object ... initialiser some_init_fn () end`

# Tools and Techniques
--- 

---
Source: https://www.goodreads.com/book/show/16087552-real-world-ocaml?ac=1&from_search=true&qid=AywbZGaVor&rank=1
