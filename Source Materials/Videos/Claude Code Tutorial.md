Tags:
- [[Agentic Engineering]]
---
## init
- `/terminal-setup`: basic setup e.g. shift-enter for newline
- `/init`: creates `CLAUDE.md` file to guide code generation

## memory
- `# ...`: persists a memory, Claude will "remember" it for each session
- `/memory`: directly edit memory files

## context
- `@path/to/file`: add a file to context
- `/exit`: to exit Claude Code session
- `/resume`: reopen an exited session
- `/clear`: clear context and chat 
- `/compact`: squash context and chat into small summary
- `<ESC><ESC>`: jump to previous point in the chat

## planning and thinking
- `<ALT><M>`: cycle to planning mode
- use the word `think` in the prompt to activate thinking

## custom slash commands
- define in `.claude/commands/*.md`
- can provide description, argument hint, and specific instructions in the md file

## subagents
- isolated agent that can be configured to work on a specialised task
- Claude Code can delegate tasks to them
- `/agents`: manage existing agents and create new ones
    - represented by markdown files in `.claude/agents/*.md`

---
Source: https://youtube.com/playlist?list=PL4cUxeGkcC9g4YJeBqChhFJwKQ9TRiivY&si=Jmm4u8zXRBGc-b6f
