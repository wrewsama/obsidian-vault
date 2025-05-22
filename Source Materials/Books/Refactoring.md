Tags:
- [[Software Engineering]]
- [[Code Quality]]
---
## Chapter 1
mainly just examples
general learning points
- set up automated tests beforehand
- break changes down into tiny steps
- test after each change

## Chapter 2
Refactoring: change made to internal structure of software to make it
- easier to understand
- cheaper to modify
WITHOUT changing its observable behaviour

Don't try to add functionality and refactor at the same time. Do it one at a time

Why:
- improve design of software
	- clearer structure
	- deduplication
- makes software easier to understand
- easier to spot bugs
- dev speed (adding features in the late game)

When:
- rule of 3: code duplicated 3 times => refactor
- preparatory: make it easy to add your feature, then add it
- comprehension: make it easier to understand
- litter-pickup: when you understand what the code is doing, but it's doing it badly

refactoring & performance optimisation: 
0. build program in a well-factored manner
1. profile the program
2. find the performance hotspots
3. focus on optimizing those
(based on the observation that for most programs, most of the time is spent in a small fraction of that code)

## Chapter 3
Code smells

- Mysterious Name
- Duplicated code
- Long function
- Long param list
- Global data
- Mutable Data
- Divergent Change: Change in requirements for different contexts require changes in the same module
- Shotgun Surgery: Change in 1 requirement requires many changes in different classes (kind of the opposite extreme of divergent change)
- Feature Envy: 1 module's function spends more time using another module's fields/functions
- Data clumps: group of data used together often (e.g. as class fields or function params)
- Primitive Obsession
- Repeated `switch`
- Loops
- Lazy Element: abstraction (class/function) that doesn't improve clarity or extensibility
- Speculative generality: shit that handles shit you don't need
- Temporary field: set only in certain circumstances
- Message chain: object gets another object which gets another object and so on
- Middle Man: Most methods in a class are just delegating to another class
- Insider Trading: modules trade too much data between them
- Large class
- Alternative class with different interfaces
- Dataclasses
- Refused bequest: subclass doesn't need parent's fields/methods
- Comments (not really a code smell, but often ends up concealing bad code)

## Chapter 4
- trying to write too many tests usually leads to not writing enough. Focus on writing tests for the parts with the highest risk of bugs
- concentrate tests on the boundary conditions under which things might go wrong

## Chapter 5
-

## Chapter 6
##### Extract Function
- when the implementation does not clearly show the intention
- extract the implementation out into a function, named after that intention
- pass required vars into the function as params
- replace other, similar code with a call to the new function
##### Inline Function
- inverse of [[#Extract Function]]
- ensure the function isn't polymorphic
##### Extract Variable
- to simplify complex expressions
- must ensure no side effects
- declare immutable variables for each part of the expression
##### Inline Variable
- inverse of [[#Extract Variable]]
##### Change Function Declaration
- rename function / edit parameters
- extract the function body into the edited one
- find and replace
##### Encapsulate Variable
- deal with mutable global variables
- just set up getters and setters and change references to the variable to the getters/setters instead
- if possible, make the variable private
##### Rename Variable
self-explanatory
##### Introduce Parameter Object
- for groups of data items that regularly appear together
	- fields of a class
	- params of a method
- replace with another data structure
##### Combine Functions into Class
self-explanatory, use when a group of functions operate on the same param
##### Combine Functions into Transform
similar to [[#Combine Functions into Class]] except we group the functions into another function
##### Split Phase
- for code that's doing multiple different things at the same time
- split into different phases and extract them into different functions
- use intermediate data structure to transfer data from 1 phase to another
## Chapter 7
##### Encapsulate Record
Wrap a data record (e.g. data in a hashmap) in a class with appropriate getters and setters
##### Encapsulate Collection
- Wrapping a collection in a class
- For the class to retain control over that collection
	- getter (for the _collection_, not for a single element) cannot return the actual collection or else clients can still modify the collection any way they want
		- either return copy or read-only proxy
	- setters (again for the _collection_) should take in a copy
##### Replace Primitive with Object
- for primitive fields/variables with too much reused logic (e.g. parsing a `string` phone number)
- replace it with an object with that logic as methods
##### Replace Temp with Query
- substitute temporary variable with a field / getter function
- avoid duplication
##### Extract Class
- when a class is too big to understand easily
- split it into different classes, each with its own responsibility
##### Inline Class
inverse of [[#Extract Class]]
##### Hide Delegate
- when an object (the server) returns another object (the delegate) to the client and the client calls a method on that delegate
- should encapsulate the delegate's method in the server object. i.e. client shouldn't have to know about the delegate
##### Remove Middleman
- when an object needs to add a wrapper method for every method added to another
- inverse of [[#Hide Delegate]]
##### Substitute Algorithm
- replacing the implementation of a function with a better one
## Chapter 8
##### Move Function
- when a function needs to access / get accessed by different contexts
- move nested function to top level
- move function from 1 class to another
##### Move Field
- when the same pieces of data always get passed into functions together
- when changing 1 record forces a change in another
- move fields to more appropriate records / classes
##### Move Statements into Function
- if the same code needs to be executed every time a function is called, that code should be moved to inside that function
##### Move Statements to Caller
- inverse of [[#Move Statements into Function]]
- when you need to make a caller do something different from other callers when calling the function
##### Replace Inline Code with Function Call
- when there is inline code that's doing the same thing as an existing function
##### Slide Statements
- group lines doing things related to each other closer together
- when sliding a line over another, ensure there is no interference
	- referencing something the other line declares
	- modifying the same element
##### Split Loop
- for loops doing multiple things at once
- split it into separate loops, each doing 1 thing
##### Replace Loop with Pipeline
- replace a loop with `map`/`filter`/`reduce`
##### Remove Dead Code
self explanatory

## Chapter 9
##### Split Variable
- when a variable is reassigned to a different value serving a different purpose
- split into 2 variables, named after their singular purpose, preferably immutable
##### Rename Field
self explanatory
##### Replace Derived Variable with Query
- derived variable: variable that can be derived from other existing variables. This is a duplication of data
- replace it with a getter function that derives the value from the other variables
##### Change Reference to Value
- within an object, store a "value" instead of a reference to another object
- when updating, replace that entire "value" object with an updated one
- ensures upstream dependencies don't affect each other by modifying the shared dependency
##### Change Value to Reference
inverse of [[#Change Reference to Value]]

## Chapter 10
##### Decompose Conditional
- when the conditional is so complicated the intention of each part is obscured
- replace the clauses and the body of the conditionals with function calls, named after their intentions
##### Consolidate Conditional Expression
- when there are several adjacent conditional blocks that do the same thing
- consolidate all the clauses into 1 clause (`||`)
- the consolidated clause is also a good candidate for [[#Decompose Conditional]]
##### Replace Nested Conditional with Guard Clauses
- when the conditional has 1 leg that's a 'normal behaviour' and another that's 'abnormal'
- check the abnormal condition and return early if true
##### Replace Conditional with Polymorphism
- when several functions switch on the same type code
- extract the bodies into their own polymorphic method
- define a default in the base class if needed
##### Introduce Special Case
- when several places check the same thing for an object
- make a special case as a subclass of that object
##### Introduce Assertion
- when a section of code can only work if some precondition is true
- used to communicate those preconditions and to debug easily

## Chapter 11
##### Separate Query from Modifier
- functions that return values should not have _observable_ side effects
- separate the query part that returns a value and the side effects into separate functions
##### Parameterise Function
- when 2 functions carry out similar logic but with some different literal values
- use 1 function with parameters for the values that differ
##### Remove Flag Argument
- flag: a literal param that's used to decide which logic to execute
```python
def foo(flag):
	if flag == 'something':
		# do something
	elif flag == 'something else':
		# do something else
	else:
		# do another thing
```
- create explicit function for each case and call that instead (e.g. `doSomething()` instead of `foo('something')`)
##### Preserve Whole Object
- for code that derives several values from a record / object, then passes those values into a function
- make the function take in the whole record / object instead
##### Replace Parameter with Query
- when 1 parameter can be derived (queried) from another parameter, take in only that parameter and derive the other
##### Replace Query with Parameter
- when function does some query on something that shouldn't be in it's scope
- inverse of [[#Replace Parameter with Query]]
##### Remove Setting Method
self-explanatory
##### Replace Constructor with Factory Function
advantages of constructor:
- can return subclass or proxy instead
- can be passed around like a normal function
- can be renamed easily
##### Replace Function with Command
- parameters -> fields of the command object
- implementation -> `execute()` method of the command object
advantages of command:
- can use the methods and fields of the object to break down a complex implementation
- can support complementary operations (e.g. `undo`)
- if using a language without functions as first-class citizens, can pass the command object around as a first class citizen
##### Replace Command with Function
- inverse of [[#Replace Function with Command]]
- when the complexity of the command object outweighs its advantages

## Chapter 12
##### Pull Up Method / Field / Constructor Body
- when all subclasses have the exact same method / field
- if the subclasses only share a part of their methods / constructors, can [[#Extract Function]], then pull it up
- pull it up to their common ancestor
##### Pull Down Method / Field
- inverse of [[#Pull Up Method / Field / Constructor Body]]
- when the method / field is only relevant to 1 subclass
##### Replace Type Code with Subclasses
- when a class has a field that serves as a "type code" and either
	- several methods switch on that type code
	- there is a field / method that is only relevant to one of the types
- direct inheritance:
	- make subclasses of the target class for each type code and implement the relevant methods / fields
- indirect inheritance:
	- used if the target class already has its own subclasses
	- make a `Type` class and store it in the target class
	- make subclasses of `Type` for each type code and implement the 'switched' methods & relevant fields
##### Remove Subclass
- inverse of [[#Replace Type Code with Subclasses]]
- when the cost of managing multiple subclasses outweighs the benefits (e.g. duplication, extensibility)
##### Extract Superclass
- when 2 classes have methods that do similar things
- create a superclass and make those classes extend it
- [[#Pull Up Method / Field / Constructor Body]] for the common things
##### Collapse Hierarchy
- merge subclass and superclass
##### Replace Subclass with Delegate
- favour composition over inheritance
	- inheritance can only be used to vary behaviour based on 1 variable
	- couples parents with children
- turn the subclass methods into delegate objects
- make a factory function for those
- make the parent class
	- contain a reference to the appropriate delegate
	- call that delegate's method in the parent method
##### Replace Superclass with Delegate
- once again, favour composition over inheritance
- when inheritance is used purely to let the subclass use the superclass's methods, but the subclass shouldn't have the same interface
- remove the inheritance relationship and store a reference to the "superclass" inside the "subclass"