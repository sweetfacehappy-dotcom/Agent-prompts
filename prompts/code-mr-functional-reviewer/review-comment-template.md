---
title: Code MR Functional Reviewer — Review Comment Template
agent_id: code-mr-functional-reviewer
prompt_type: output-template
status: draft
created: 2026-07-08
updated: 2026-07-08
framework: ../../frameworks/code-mr-functional-reviewer.md
---

# Code MR Functional Reviewer — Review Comment Template

This is a separate reusable output template for MR comments produced by the Code MR Functional Reviewer.

```markdown
## Functional MR Review

**Verdict:** `<functionally_approve | comment_only | request_changes>`<br>
**Confidence:** `<high | medium | low>`<br>
**Jira ticket:** `<Jira key or URL>`<br>
**MR:** `<MR URL or ID>`

### Summary

<2-5 sentences. Focus on whether the MR satisfies Jira acceptance criteria and project documentation.>

### Acceptance Criteria Traceability

| ID | Source | Expected behavior | Implementation evidence | Test evidence | Status | Notes |
|---|---|---|---|---|---|---|
| AC-1 | Jira | `<criterion>` | `<file:line or missing>` | `<test file:line or missing>` | `<met/partially_met/not_met/unclear/conflict>` | `<short note>` |

### Project Documentation Checked

| Document | Relevant expectation | Result |
|---|---|---|
| `<path or URL>` | `<documented rule>` | `<consistent/conflict/not applicable>` |

### Blocking Findings

- None

### Warnings and Suggestions

- None

### Open Questions

- None

### Limitations

- None
```
