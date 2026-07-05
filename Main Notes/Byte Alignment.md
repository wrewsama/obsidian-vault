Tags:
- [[Computer Architecture]]
---
## what
- arranging fields in data structures such that they match the appropriate byte boundaries
- avoids compiler adding extra padding to avoid the extra fetch cycles
- padding is added between fields to ensure each field starts at an address that's a multiple of its size
- padding is also added at the end to ensure the struct's size is a multiple of the biggest field
## how
```c
struct BadOrder {
    char a; // pad 3 bytes
    int b;
    char c; // pad 3 bytes
};

struct BetterOrder {
    int b;
    char a;
    char c; // pad 2 bytes
};

int main() {
    struct BadOrder bad;
    struct BetterOrder better;
    printf("Size: %lu bytes\n", sizeof(BadOrder)); // 12
    printf("Size: %lu bytes\n", sizeof(BetterOrder)); // 8
    return 0;
}
```

---
## References
- https://www.naukri.com/code360/library/structure-padding-in-c
- https://medium.com/@yewintt.naing/understanding-memory-alignment-and-struct-padding-a4cbf11cb9c3