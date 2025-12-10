Tags:
- [[Software Engineering]]
---
## What is SWE
- $SWE = \int programming\ dt$
- SWE involves more time, scale, and trade-offs than simple programming
    - time: software that needs to be used over a long period of time will need to be subject to a lot of change
    - scale: if the cost of an operation grows superlinearly over time, it is not scalable
    - trade-offs: engineering decisions should be based on
        - we must (due to legal or customer requirements)
        - it's the best, based on the current evidence
> **Hyrum's Law**: With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviours of your system will be depended on by somebody

## How to work well in teams
>**The Genius Myth:** human tendency to ascribe success of a team to a single person
- Don't try to be the lone genius who does all the work by himself
- Share what you're working on with others 
    - get early feedback
    - ensure knowledge is well dispersed
- the 3 pillars of social interaction
    - humility
    - respect
    - trust
- Don't purposely contradict the system because of your ego, use it and learn how to adapt the system to your desires
- "Googleyness": Google's desirable personality traits
    - thrives in ambiguity
    - values feedback
    - challenges status quo
    - puts the user first
    - cares about the team
    - does the right thing

## How to lead a team
- lose the ego
    - trust in your team's competence
    - appreciate feedback and questions from team members
    - apologise when you've made a mistake
- be Zen
    - exude calm in all situations
    - respond to questions with questions: the person asking for help on a problem doesn't want you to solve the problem, just help them be able to solve it themselves
- be a catalyst
    - build consensus within the team
- remove roadblocks
- be a teacher and mentor
    - requirements: experience with team's systems, ability to explain, **ability to gauge how much help is needed**
    - the last one is the most important
- set clear goals
- be honest
-  track happiness
    - make sure team members get their deserved recognition and are happy with their work

## Leading at Scale
- **Always Be Deciding**
    - identify blinders: use a fresh pair of eyes to look at the problem, ask questions, and consider new strategies
    - identify trade-offs
        - Good, Fast, Cheap: pick 2
    - make a decision and iterate on it
- **Always Be Leaving**
    - don't be a single point of failure
    - build a self-driving team
    - divide the problem, delegate subproblems, iterate as needed
    - NOTE: a team should be in charge of a _problem_, not a _product_
- **Always Be Scaling**
    - spiral of success
        - struggle > traction > compress > bigger struggle > repeat
    - drop / delegate non-important tasks
    - block out time for important tasks only

## Goals/Signals/Metrics Framework
- goal: desired end result
- signal: how you can know the end result has been achieved
- metric: something that you can actually measure that implies the signal

## Code Review
- correctness
- comprehensibility
- consistency (with the rest of the codebase)

## Testing
- test size
    - small: run in a single process
    - medium: multiple processes, single machine
    - large: run in multiple machines
- write the smallest sized test required for a given piece of functionality
- test scopes and % of test case count
    - 5% end to end
    - 15% integration
    - 80% unit
> **Beyonce Rule**: "If you liked it, you shoulda put a test on it"

## Unit Testing Guidelines
- Write tests that won't need to be changed
    - even after code refactoring, bugfixes, or adding new features
- test public APIs, not implementation details
- test for the desired state, not for interactions with other components
- test behaviours, not whole methods
    - each test case should follow the format: given {precondition}, when {method}, then {assert some behaviour}
- don't put logic in tests
    - no control flow, loops, etc.
- write clear failure messages
- tests should be DAMP, not DRY
    - "descriptive and meaningful phrases"
    - essentially, okay to duplicate if it makes the test clearer to read and understand by avoiding extra indirection

---
Source: https://www.goodreads.com/book/show/48816586-software-engineering-at-google
