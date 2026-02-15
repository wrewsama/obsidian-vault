Tags:
- [[System Design]]
---
## what
- interactive widget that ChatGPT can invoke
- let's the user interact with things outside ChatGPT e.g. hotel booking, google drive

## how it works
```
+-----------------------+
|     ChatGPT (Host)    |
|  - Renders apps       |
|  - Manages state      |
|  - Calls tools        |
+-----------------------+
          |  ^
   calls  |  |  returns
   tools  v  |   data
+-----------------------+
|     MCP Server        |
|     (Backend)         |
|  - Defines tools      |
|  - Serves UI          |
|  - Business logic     |
+-----------------------+
          |  ^
   sends  |  |  user
    UI    v  |  actions
+-----------------------+
|       Widget          |
|      (Frontend)       |
|  - React app          |
|  - Sandboxed iframe   |
+-----------------------+
```
- MCP server
    - define resources so GPT can load html templates (compiled react app)
    - define tool that GPT to call to invoke the app
        - returns data and which html template to load
- widget
    - runs in a sandboxed iframe
    - ChatGPT injects a `window.openai` object, allowing the app to interact with ChatGPT
        - call other tools
        - get and set state for the widget
        - send follow-up messages to GPT
---
Source: https://newsletter.systemdesign.one/p/apps-in-chatgpt
