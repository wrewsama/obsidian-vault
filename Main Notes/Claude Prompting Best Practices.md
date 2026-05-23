Tags:
- [[Agentic Engineering]]
---
## principles
- be specific and clear
- provide context behind the instruction (`do XXX because it's needed for YYY`)
- use a few-shot prompt with examples in `<example> </example>` XML-esque tags
- give Claude a role in the system prompt (`you are a ...`)
- put long docs and info at the top of the prompt and the query / instructions at the bottom, e.g.
```
<documents>
  <document index="1">
    <source>annual_report_2023.pdf</source>
    <document_content>
      {{ANNUAL_REPORT}}
    </document_content>
  </document>
  <document index="2">
    <source>competitor_analysis_q2.xlsx</source>
    <document_content>
      {{COMPETITOR_ANALYSIS}}
    </document_content>
  </document>
</documents>

Analyze the annual report and competitor analysis. Identify strategic advantages and recommend Q3 focus areas.
```
- tell Claude what to do instead of what not to do
## other info
- adjust thinking depth with the `effort` parameter. Default to `xhigh` for coding and intelligent agents
- `high` / `xhigh` effort settings use tools more often, but you can specifically prompt lower effort settings to do so too
- you can give explicit guidance on the use of subagents
- the same can be done for parallel tool calls

## long running workflows
- use the first context window to set up a framework
    - test suites
    - setup scripts
    - checklists
- clear after each context window is filled
- keep task state in a structured format e.g. `tests.json` to store which tests are passing, failing, and not started (useful for TDD)
- use unstructured text for progress notes (`.md`, `.txt`)

---
Source: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
