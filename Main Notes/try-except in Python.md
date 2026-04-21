Tags:
- [[Python]]
---
## try
- contains the code to execute
## except
- executes when the try block raises an exception that matches the one in this except block
- e.g. `except ValueError, RuntimeError` will catch **any subtype** of `ValueError` or `RuntimeError`
    - if there are multiple except blocks that match, the first will execute, even if it's less specific
- can re-raise the caught exception with a bare `raise`
- or _chain_ the exception with `raise OtherException from exc`
## else
- executes when the try block finishes **completely**
- if the try exits with `return`, `break`, or `continue`, `else` isn't executed
## finally
- always executes as the last task before the whole try/except/else statement completes
- if an uncaught exception occurs (not caught by `except` or raised in an `except` / `else` block)
    - `finally` block will execute and re-raise the exception
    - however, if `finally` raises an exception or executes a `return`, `break`, or `continue`, it **will not re-raise the exception**
- if the `try` statement reaches a `return`, `break`, or `continue`, the `finally` clause will execute just before it
    - once again, if the `finally` clause itself executes a `return`, `break`, or `continue`, we won't go back to executing the `try`'s statement
    
- in a nutshell, finally will always run, even if `try` exits abruptly (`Exception`, `return`, `break`, or `continue`). If `finally` exits abruptly too, it ignores the `try`'s abrupt exit
---
## References
- https://docs.python.org/3/tutorial/errors.html#handling-exceptions