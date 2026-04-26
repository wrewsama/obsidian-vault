Tags:
- [[Python]]
---
- implemented as a `PyLongObject` struct
- splits the binary representation into 30 bit blocks and stores them as elements in an array
    - in other words, the array is just the digits of the base $2^{30}$ representation
- can grow arbitrarily large (just grow the array like a list)
- can represent with different bases e.g. `0b`, `0o`, `0x`, but printing will always be base 10 (e.g. `print(0b100) # prints 4`)
- **gotcha**: string->int conversions are limited to 4300 characters
    - can be bypassed with `sys.set_int_max_str_digits`
---
## References
- https://arpitbhayani.me/blogs/long-integers-python/
- https://stackoverflow.com/questions/73693104/valueerror-exceeds-the-limit-4300-for-integer-string-conversion