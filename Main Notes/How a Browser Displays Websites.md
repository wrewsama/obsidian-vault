Tags:
- [[Computer Networking]]
---
1. [[HTTP Request Process]] (browser sends HTTP request and receives HTML page)
2. In the main thread, the HTML is parsed and the Document Object Model (DOM) tree is built
3. At the same time in a separate thread, the preload scanner scans the available content and requests resources (CSS, JS, fonts, images, etc) in the background 
4. The CSS is parsed and the CSS Object Model (CSSOM) Tree is built
5. JS is parsed into abstract syntax trees, then interpreted & JIT compiled by the browser's JavaScript engine
6. DOM and CSSOM are combined into a render tree
7. The layout (size and position of each object) is determined
8. The individual nodes are painted (converting each box in the layout into pixels on the screen)

---
## References
- 