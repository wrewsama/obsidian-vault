Tags:
- [[Software Engineering]]
- [[Code Quality]]
---

|               | Assertions                                                  | Exceptions                                                                                             |
| ------------- | ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Purpose       | Ensure all assumptions in the code are adhered to           | Handle abnormal conditions gracefully, creating an alternative to the happy flow                       |
| Use case      | Programmer errors in the code (e.g. null pointer deference) | Exceptions due to issues outside the programmer's control (e.g. connection terminated, file not found) |
| Behaviour     | Terminates program immediately                              | Can be caught and handled                                                                              |
| In Production | Can be disabled                                             | Permanent                                                                                              |

---
Source:
