Tags:
- [[Software Engineering]]
- [[Spring]]
---
- Pattern used to implement IoC
- We _inject_ the object's dependencies externally instead of having the object create it internally
- Reduce coupling between object and its dependencies

Example:
```java
// without DI
public class Store {
    private Field f;
    public Store() {
        this.f = new Field();
    } 
}

// with DI (in particular, constructor injection)
public class Store {
    private Field f;
    public Store(Field f) {
        this.f = f;
    } 
}
```

This can be achieved through:
- constructor injection
- field injection (only with Spring `@Autowired` annotation)
- setter injection
- method injection

---
## References
- 