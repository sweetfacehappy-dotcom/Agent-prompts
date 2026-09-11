# Files & Folders Personal-Agent Harness

## Purpose

A lightweight operating framework for a personal AI agent being built from scratch. It uses the workspace filesystem as a durable, human-readable harness around the model instead of relying on one giant system prompt or a custom multi-agent framework by default.

## Origin and interpretation

This framework is adapted from Jake Van Clief's video, [*Stop Building AI Agents. Use This Folder System Instead.*](https://www.youtube.com/watch?v=MkN-ss2Nl10). The accompanying course adaptation is [Files & Folders](https://files-and-folders-six.vercel.app), and the original adoption artifact was `AGENTS-FILES-AND-FOLDERS.md`.

It should be treated as a practical synthesis, not as a transcript or claim that every workflow can be solved with folders alone.

## Operating context

The model is the reasoning engine. The harness supplies:

- durable context and state;
- routing and progressive disclosure;
- task-specific capabilities and integrations;
- explicit permissions and approval gates;
- verification and recovery;
- readable artifacts and change history.

The central design choice is to start with plain folders and Markdown because they are portable, diffable, inspectable, and easy for both humans and agents to edit. Add databases, services, or specialized orchestration only when the workflow demonstrates that files are no longer sufficient.

## Three layers

1. **Root router** — identity, domain map, conventions, task routes, safety boundaries.
2. **Workspace context** — local purpose, process, read/skip rules, standards, and outputs.
3. **Task capability** — skills, MCP, references, tools, tests, and integrations loaded only for the route that needs them.

## Inputs

- A user request or task brief.
- A root router such as `AGENTS.md`.
- A matching workspace context file.
- Relevant briefs, specs, source notes, decisions, and prior artifacts.
- Optional task-scoped tools and capabilities.

## Outputs

- A named artifact in the correct workspace stage.
- Verification evidence appropriate to the artifact.
- A concise change receipt: what changed, where, checks run, limitations, and follow-ups.
- Durable updates to decisions, plans, or context when the work changes the system.

## Assumptions

- The human can inspect and edit the workspace.
- File names and folder boundaries are meaningful enough to support routing.
- The agent can search and read files before acting.
- Not every task requires every capability.
- Consequential external actions have controls outside the model.

## Design rules

- Route by real work domain, not by tool brand.
- Prefer progressive disclosure over global context loading.
- Keep root instructions short and local context specific.
- Use explicit pipeline folders such as `briefs/`, `specs/`, `builds/`, and `outputs/` where appropriate.
- Use naming conventions as lightweight retrieval.
- Keep source, active work, generated artifacts, and runtime state separate.
- Test the system with real requests; improve the route when the agent reads too much, too little, or the wrong material.
- Treat repeated failures as evidence for improving the harness, not merely for adding adjectives to the prompt.

## What this is not

- Not a replacement for databases when the workflow needs transactions, concurrency, permissions, or structured querying.
- Not a replacement for application code or specialized agents in every domain.
- Not permission to let the agent publish, delete, spend, or modify external systems without explicit gates.
- Not a reason to load all files, skills, or MCP tools on every request.

## Maintenance

Review the router and workspace context after real use. When a failure repeats, update the smallest relevant durable control—route, context note, skill, checklist, test, permission, or recovery rule—and verify the improvement with another bounded task.

The paste-ready operational prompt is at [`prompts/files-and-folders-personal-agent/system-prompt.md`](../prompts/files-and-folders-personal-agent/system-prompt.md).