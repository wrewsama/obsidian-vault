Tags:
- [[Software Engineering]]
- [[SRE]]
- [[Debugging]]
---
## How Failures Come to Be
- clearer terminology
    - **defect**: incorrect program code
    - **infection**: incorrect state of the running program
    - **failure**: observation incorrect behaviour
- TRAFFIC process
    - Track problem
    - Reproduce the failure
    - Automate and simplify the failing test case
    - Find possible infection origins
    - Focus on most likely origins
    - Isolate infection chain
    - Correct defect
## Tracking Problems
TODO

## Making Programs Fail
TODO

## Reproducing Problems
TODO

## Simplifying Problems
TODO

## Scientific Debugging
- systematic process to isolate the cause of a failure
    - observe
    - create hypothesis of the root cause 
    - make predictions (observable result) on that hypothesis
    - test hypothesis with experiments and observation
    - if experiment satisfies predictions, refine the hypothesis
    - else, create new hypothesis that can explain all previous observations
    - repeat until hypothesis can't be refined

## Deducing Errors
- goal: from looking at the code, _deduce_ where infections originate from
- trace the dependency graph
    - edge between line X and line Y if X modifies some variable that's read by Y AND there exists a path from X to Y in the **control flow graph**
    - forward slices: everything dependent on the current line
    - backward slices: everything the current line depends on

## Observing Facts
- goal: determine facts about what happened in a run
- checking logs
- debuggers
    - using cli tools like GDB to run code or check core dumps
    - hooking into the interpreter using `sys.settrace(tracer_fn)` to register a tracer callback that fires every time a line is executed

## Tracking Origins
TODO

## Asserting Expectation
TODO

## Detecting Anomalies
TODO

## Causes and Effects
- to verify if X causes Y (X <=> Y), check both cases (X true and X false)
- to find _a_ cause
    - set hypotheses for possible causes
    - verify each one
- to find _the actual_ cause
    - get the difference between the actual world and the closest possible world without the effect
- common causes
    - input
    - state
    - code

## Isolating Failure Cases
TODO

## Isolating Cause-Effect Chains
TODO

## Fixing the Defect
TODO

## Learning from Mistakes
TODO

---
Source: https://www.goodreads.com/book/show/6882295-why-programs-fail
