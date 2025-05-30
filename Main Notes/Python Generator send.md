Tags:
- [[Python]]
---
## What it is
- feature of [[Python Generators]]
- allows for 2-way communication between the generator and its caller
- value passed to `send` is the return value of `yield` in the generator
- `x = yield y` => `next(generator)` returns `y` and `generator.send(val)` sets `x = val`

## Example - accumulator
```python
def accumulator():
    total = 0
    while True:
        value = yield total  # yield current total and wait for new value
        if value is not None:
            total += value  # update total with sent value

acc = accumulator()
next(acc)          # Start the generator, yields 0
print(acc.send(10))  # Send 10, total becomes 10, yields 10
print(acc.send(5))   # Send 5, total becomes 15, yields 15
print(acc.send(20))  # Send 20, total becomes 35, yields 35
```
---
## References
- 