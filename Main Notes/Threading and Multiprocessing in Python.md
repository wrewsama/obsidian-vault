Tags:
- [[Python]]
---
- create a thread: `t = threading.Thread(target=<FUNCTION>, args=<TUPLE OF ARGS>, kwargs=<DICT OF KWARGS>)`
- start running a thread: `t.start()`
- wait for a thread to complete: `t.join()`

> Note: Python `threading` using **kernel threads** [[Kernel VS User Threads]]

|                     | threading                                                                                | multiprocessing                                 |
| ------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------- |
| create              | `t = threading.Thread(target=<FUNCTION>, args=<TUPLE OF ARGS>, kwargs=<DICT OF KWARGS>)` | `p = multiprocessing.Process`, args are similar |
| start               | `t.start()`                                                                              | `p.start()`                                     |
| wait for completion | `t.join()`                                                                               | `p.join()`                                      |

---
## References
- 