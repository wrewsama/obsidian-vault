Tags:
- [[Software Design]]
- [[Software Engineering]]
---
## Software Development Metaphors
- metaphors model the development process, 
- they provide a heuristic to guide your choices
- some common metaphors
    - software development: constructing a house
    - software techniques: toolbox

## Prerequisites
- project phases
    - **prerequisites**
    - construction
    - testing
- 2 main prerequisites
    - requirement gathering
    - architecture design
- significance
    - avoid cost of building the wrong thing
    - useful even when doing iterative development
- requirements checklist
    - functional requirements
        - tasks
            - inputs
            - outputs
        - external interfaces
    - non-functional requirements
        - latency
        - security
        - reliability
        - resource usage
        - maintainabilty: ability to adapt to changes in functionality, external interfaces, environment, etc.
    - quality
        - any conflicts
        - consistent level of detail
        - clarity
        - relevance
        - testability
        - completeness: if all requirements are satisfied, is product "done"
- architecture
    - system-wide design overview
    - major classes (subsystems)
    - data design (data structures, schemas)
    - UI design
    - resource usage estimates / resource management solutions
    - estimates & how to handle
        - security
        - performance
        - scalability
        - interoperability
        - i18n
    - error handling
    - feasibility
    - change strategy (most likely changes should be easy to implement)
> **Conceptual Integrity**: You should be please by how natural and easy the solution seems. It shouldn't look as if the problem and the architecture have been forced together with duct tape.

## Design in Construction
- Software design is a "wicked problem": It can only be clearly defined by solving it
> Even if we could invent a programming language that used the same terminology as the real-world problem we're trying to solve, programming would still be difficult because of the challenge in determining precisely how the real world works
- **Primary goal of design: Managing Complexity**
    - Essential Complexity
        - unavoidable complexity arising from interfacing with the complex, real world
        - need to minimise the amount that needs to be considered together at any point in time
    - Accidental complexity
        - complexity that arises from the production process (e.g. from dependency management, microservices, etc.)
        - prevent them from proliferating needlessly, abstract it away and use that abstraction
- Design Heuristics 
    - Model with the objects in the real world
    - Stratify abstractions
    - Encapsulate and hide information
        - each class, module, or package is defined by the information it _hides_
        - ask "what should this class _hide_" instead of what should it _have_
    - Use inheritance <=> it simplifies the design
        - e.g. polymorphism to allow dependent classes to depend on a general class, without caring about the specific dependency
    - Anticipate areas likely to change
        - separate each area into its own class
        - isolate it by defining an interface that can remain the same after the change
    - Keep coupling loose
        - relationships between classes should be small, direct, visible, and flexible
        - ensure classes don't rely on implementation details of other classes (also information hiding)
    - Use common Design Patterns
    - Other heuristics
        - strong cohesion: everything in a class/module/package has a singular central purpose
        - formalise class contracts: preconditions and postconditions
        - assign responsibilities
        - design for testing
        - avoid failure: think about possible failure cases for the system and design to avoid them
        - choose binding time consciously: bind value to variable early => simplicity; later => flexibility
        - central points of control
        - consider brute force
        - draw a diagram
    - Design practices
        - Iterate
        - Divide and Conquer
        - Top-Down or Bottom-Up
            - top-down: start from rough general idea, decompose into specifics
            - bottom-up: start from specific requirements, generalise into a simple solution
        - Experimental Prototyping
        - Collaborative Design

## Working Classes
- **Abstract Data Type (ADT)**: Collection of
    - data
    - operations on that data
- A class should represent exactly 1 ADT, not just arbitrary collections of data and methods
- ensure encapsulation
    - hide as much as possible
    - beware of semantic violations: when the methods require knowledge of the implementation details to be used (e.g. if `Foo::a` needs `Foo::b` to be run first)
- "Has a" relationships
    - prefer containment over inheritance
    - try not to exceed 7 members in a class
- "Is a" relationships
    - adhere to Liskov Substitution Principle - superclasses must be able to be _substituted_ for any of their subclasses without the user needing to know the difference
    - avoid deep inheritance trees - aim for around 2-3 levels
    - move common behaviour as high up the inheritance tree as possible
    - prefer polymorphism to switching + type checking
- methods
    - keep as few as possible
    - minimise extent to which the class interacts with other classes
        - instantiation of other objects
        - method calls on other objects
- constructors
    - init all member data in constructor
    - prefer deep copies unless absolutely required

## High Quality Routines
- Reasons to create routines
    - hide complexity
    - avoid duplication
    - force sequences
        - e.g. if action A needs to be done only after action B, put them one after the other in 1 routine and make sure A is only accessible through that routine
- ideal routine: **functional cohesion**: performs one and only one operation
- naming guidelines
    - describe everything the routine does
    - avoid vague terms (e.g. `handleX`, `processY`)
    - be consistent with opposites (e.g. `add_item/remove_item` instead of `add/remove_item_from_list`)

## Defensive Programming
- assertions
    - Handle expected errors, `assert` for errors that should never occur
    - `assert` preconditions that should always be true
    - don't put application logic into assertions, they may get removed during prod builds
- exceptions
    - throw only for exceptional cases
    - throw at the correct level of abstraction (e.g. `Cohort::getStudents` shouldn't throw `ArrayOutOfBoundsException`, this reveals the implementation details of the class)
    - when throwing, include all info that led to it
- other error handling
    - log warning/error message
    - sub in another value (e.g. default, previous, next, closest, etc.)
    - return error code
    - abort
- dev builds can have extra debug aids than prod builds, they don't have the same resource/performance constraints
- what defensive programming to leave in prod code
    - important error checks
    - error logs
    - graceful shutdowns

## Pseudocode Programming Process (PPP)
for designing and implementing a routine
- name it and define what it's trying to do
- check if it's already implemented elsewhere (if yes just use that instead) 
- write pseudocode in comments
- review and improve the pseudocode
- implement below the comments
- test the code
- review and repeat the process, iterating as needed

## General Issues in Using Variables
- guidelines
    - initialise variables when they are declared as much as possible (i.e. `int x = 5` instead of `int x`)
    - initialise & declare close to where it's used
    - initialise member data in constructor 
    - minimise "lifetime" of variable
        - lines between 1st and last usage
        - average number of lines between adjacent usages
        - (not referring to the memory lifetime)
        - can be done through grouping lines that use the same variable(s) together in their own functions
    - use each variable for exactly 1 purpose

## Variable names
- most important: should fully describe what it represents
- avoid:
    - different variables that have names that look/sound similar or mean similar things
    - shadowing standard types, variables, and routines (e.g. `date`, `int`)

## Organising Straight-Line Code
- make order-dependencies obvious (e.g. by naming, by making previous steps return values passed into later steps, or simply by commenting)
- group related statements

## Using Conditionals
- ordering
    - put normal/most frequent cases before exceptional cases
        - unless you can use guard clauses
        - in any case, keep the ordering consistent
    - if cases are equally important, order lexicographically
- keep actions in each case simple, extract into a routine if needed
- explicitly check each case, reserve the `default`/`else` case for legit defaults or exceptions, don't leave your last case to be handled in the `default`/`else` block

## Table Driven Methods
- can replace complicated `if/else` chains and inheritance structures with lookup tables

## Control Issues
- boolean conditions
    - try to avoid checking for negatives (e.g. `if not flag:`)
    - keep inequalities in number-line order (e.g. `if (min <= x) and (x <= max)`)
- deep nesting
    - flatten to a single `if/else` chain or `select/case`
    - extract the nested code into its own routine
    - use polymorphic objects + factory method
    - use guard clauses

## Software Quality
- external metrics (user POV)
    - correctness
    - usability
    - efficiency
    - reliability
    - integrity
    - adaptability (without modification)
    - accuracy
    - robustness
- internal metrics (dev POV)
    - maintainability
    - flexibility
    - portability
    - re-usability
    - readability
    - test-ability
    - understand-ability
> **General Principle of Software Quality**: Improving quality decreases development costs

##  Collaborative Construction
- pair programming
- formal inspections

## Developer Testing
- what to test
    - relevant requirements
    - relevant design concerns
    - common errors you've made
- how to improve testing
    - invest time in it
    - automate it
    - record observations of errors and adapt based on that

## Debugging
1. reproduce the error
    1. find the simplest input that still results in the error
2. locate the error source
    1. analyse data about the defect
    2. form hypothesis about what/where the defect is
    3. test hypothesis, either by testing program or reading code
    4. repeat until hypothesis is proven
3. fix the error
4. test the fix
5. look for similar errors

## Refactoring
- honestly not much here that isn't in [[Refactoring]]

## Code-Tuning Strategies
- develop the working software first, ensuring it's designed to minimise complexity (don't prematurely optimise)
- only if performance doesn't meet requirements:
    - profile the system and find slow areas
    - review choices of design, data types, and algorithms. Redesign as needed.
    - if no more design improvements can be made, tune the code of the identified bottlenecks
        - make the change
        - measure the improvement
        - if the change doesn't improve, revert it
## Code-Tuning Techniques
- conditional logic
    - order cases by frequency - minimises cases to evaluate
    - check performance of different logic structures: e.g. if/else vs case
    - use table lookups for complex boolean logic
    - use lazy evaluation
- loops
    - fuse loops together
    - unrolling: decrease number of iterations by handling more elements in each iteration
    - minimise work inside loops (extract as much as possible outside the loop)
    - with nested loops, put the busiest loop on the inside
- data transformations
    - `int` instead of `float`
    - minimise array dimensions (e.g. 1d array better than 2d array, even with same total number of elements)
    - minimise repeated array references to the same thing, store in variable instead
    - cache
- expressions
    - use algebra to simplify
    - use cheaper operations (e.g. `a+a` instead of `a*2`, `b*b` instead of `b**2`)
    - use one type per variable, avoid implicit type conversion
    - precompute results instead of duplicating expensive operations
- inline routines
- re-code in lower level language

## Managing Construction
- set coding standards
- set objectives and measure them regularly

## Integration
- the combination of separate components into a single system
- top-down integration: start from the top (i.e. `main()`), then write + integrate your way down each level of abstraction
- bottom-up integration: start from the bottom (base level classes), then write + integrate your way up
- sandwich integration: alternate between top-down and bottom-up
- risk-oriented integration: write + integrate the riskiest parts first
- feature-oriented integration: write + integrate feature-by-feature, starting with a feature that can act as a skeleton for other features
- T-shaped integration: top-down for a single slice of the system, ensure the design works end-to-end, then work on the breadth

## Self-Documenting Code
- the main contributor to code-level documentation is code written with good style
- comment guidelines
    - don't use fancy styles that are difficult to modify
    - comment on the _why_, not the _how_
    - document surprising or tricky parts

## Personal Character
- the more humble you are, the faster you'll improve
- curiosity
    - about successful projects
    - about the development process
    - about problem solving
    - conduct experiments
    - read
- intellectual humility
    - admit you don't understand, then find out, don't try to compile a program to "see what happens"
    - give accurate status reports / estimations
- Cooperate. Programming is communicating with another programmer first and communicating with the computer second
- be disciplined with standards and conventions
- be "lazy"
    - automate toil
    - know when to give up and come back to the problem later
- build the right habits by doing things right

## Themes in Software Craftsmanship
- **Conquer Complexity**
- Pick your Process: the software development process matters more than the team's individual skills
- Write Programs for People First, Computers Second: optimise for readability first
- Program _into_ Your Language, Not _in_ it: don't let the language's features dictate your coding standards
- Focus Your Attention with the Help of Conventions
- Program in Terms of the Problem Domain: top-level layers of abstraction should be in the problem domain, not language primitives, operations, and data types
- Watch for Falling Rocks: Don't ignore warnings
- Iterate, Repeatedly, Again and Again
- Thou Shalt Rend Software and Religion Asunder: Don't dogmatically stick to some methodology or standard for no good reason

---
Source: https://www.goodreads.com/book/show/4845.Code_Complete
