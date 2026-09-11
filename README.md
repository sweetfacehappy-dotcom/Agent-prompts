# Agent Prompts

Structured repository for AI agent prompts and their surrounding frameworks.

## Repository conventions

- Store every prompt as a **separate Markdown document** under `prompts/`.
- Store framework/context documents under `frameworks/`.
- Framework documents should explain the purpose, operating context, inputs, outputs, assumptions, and maintenance notes well enough to be understood six months later.
- Link prompt documents and framework documents to each other.
- Do not store secrets, credentials, or customer-sensitive raw data in this repository.

## Index

| Agent / framework | Framework | Prompt(s) |
|---|---|---|
| Code MR Functional Reviewer | [`frameworks/code-mr-functional-reviewer.md`](frameworks/code-mr-functional-reviewer.md) | [`prompts/code-mr-functional-reviewer/system-prompt.md`](prompts/code-mr-functional-reviewer/system-prompt.md) |
| Files & Folders Personal-Agent Harness | [`frameworks/files-and-folders-personal-agent.md`](frameworks/files-and-folders-personal-agent.md) | [`prompts/files-and-folders-personal-agent/system-prompt.md`](prompts/files-and-folders-personal-agent/system-prompt.md) |
