---
title: Code MR Functional Reviewer Framework
agent_id: code-mr-functional-reviewer
status: draft
created: 2026-07-08
updated: 2026-07-08
prompt_documents:
  - ../prompts/code-mr-functional-reviewer/system-prompt.md
owners:
  - Carsten
---

# Code MR Functional Reviewer Framework

## Purpose

Design an AI agent that functionally reviews code merge requests (MRs) against:

1. the acceptance criteria from a linked Jira ticket; and
2. the relevant project documentation.

The agent's job is **not** to do a generic code-quality review. Its primary question is:

> Does this MR implement the intended business/product behavior, as described by Jira and project documentation, without introducing obvious functional gaps?

## Why this exists

Human reviewers often check readability, style, and local correctness, but miss whether the MR actually satisfies the ticket. This agent creates a repeatable review pass focused on functional alignment:

- acceptance criteria coverage;
- consistency with product/domain documentation;
- missing scenarios or edge cases;
- mismatches between implementation, tests, and stated behavior;
- evidence-based feedback that can be posted back to the MR.

## Scope

### In scope

- Read Jira ticket title, description, acceptance criteria, comments, and linked issues when available.
- Read MR title, description, diff, changed files, and relevant unchanged context files.
- Read project documentation relevant to the change, for example `README`, `docs/`, ADRs, API docs, domain model docs, user-flow docs, and test strategy docs.
- Build an acceptance-criteria traceability matrix.
- Identify fulfilled, partially fulfilled, unfulfilled, unverifiable, and conflicting criteria.
- Review whether tests demonstrate the required behavior.
- Produce a review recommendation: approve functionally, comment, or request changes.

### Out of scope

- Full security audit.
- Deep performance review unless performance is part of the acceptance criteria or project docs.
- Formatting or style-only feedback unless it creates functional ambiguity.
- Rewriting the MR.
- Approving production release readiness on its own.

## Operating model

The agent runs as a review assistant in a source-control workflow. It can be used manually by a reviewer or automatically as a CI/MR bot.

Typical flow:

1. Receive MR identifier and linked Jira ticket key.
2. Fetch Jira context.
3. Fetch MR metadata and diff.
4. Discover relevant project documentation.
5. Build a compact understanding of expected behavior.
6. Map changed code and tests to each acceptance criterion.
7. Produce findings with evidence and a final functional verdict.
8. Optionally post a summary comment to the MR.

## Required inputs

| Input | Required | Notes |
|---|---:|---|
| MR URL or ID | Yes | GitLab MR or GitHub PR equivalent. |
| Repository checkout or API access | Yes | Needed for diff and surrounding files. |
| Jira ticket key or URL | Yes | If not explicitly supplied, infer from branch/MR title only when reliable. |
| Project documentation paths | Preferred | If not supplied, discover likely docs. |
| Base branch | Preferred | Defaults to MR target branch. |
| Review policy | Preferred | Defines whether the agent may post comments or only produce a report. |

## External systems

| System | Purpose | Minimum access |
|---|---|---|
| GitLab/GitHub | MR metadata, diff, comments, changed files | Read MR and repository; write comments if posting is enabled. |
| Jira | Ticket title, description, acceptance criteria, comments, linked issues | Read ticket. |
| Repository docs | Product/project behavior references | Read repository contents. |
| CI/test results | Evidence of behavior validation | Read pipeline/check results. |

## Documentation discovery strategy

If explicit documentation paths are not supplied, inspect likely sources in this order:

1. MR description links and mentioned docs.
2. Jira ticket links and mentioned docs.
3. Changed file paths and neighboring docs, e.g. `docs/feature-x.md` for `src/feature-x/`.
4. Repository-level docs: `README.md`, `docs/`, `adr/`, `architecture/`, `api/`, `product/`, `domain/`.
5. Test files and fixtures that encode expected behavior.

The agent must state which documentation it used. If no relevant documentation is found, it must say so and avoid pretending documentation validation was performed.

## Review method

### 1. Normalize the expected behavior

Extract from Jira:

- user story or problem statement;
- acceptance criteria;
- business rules;
- edge cases;
- explicit non-goals;
- dependencies and linked tickets.

Extract from docs:

- domain terminology;
- required workflows;
- API contracts;
- data model constraints;
- feature flags/configuration;
- error handling expectations;
- compatibility or migration notes.

### 2. Build traceability

For every acceptance criterion, record:

- criterion ID;
- expected behavior;
- implementation evidence in code;
- test evidence;
- documentation consistency;
- status;
- notes/questions.

Statuses:

- `met` — clear implementation and sufficient evidence;
- `partially_met` — implementation exists but has gaps or weak evidence;
- `not_met` — expected behavior appears absent or contradicted;
- `unclear` — cannot verify from available context;
- `conflict` — Jira and docs disagree, or implementation follows one but violates another.

### 3. Evaluate tests functionally

Check whether tests cover:

- each acceptance criterion;
- main happy path;
- documented edge cases;
- error and permission paths;
- regression scenarios mentioned in Jira/comments;
- relevant API/UI contract changes.

Do not require tests mechanically for every line of code; focus on whether the required behavior is demonstrated.

### 4. Produce findings

Findings must be evidence-based and actionable.

Each finding should include:

- severity: `blocking`, `warning`, or `suggestion`;
- source: Jira criterion, project doc, or inferred functional risk;
- evidence: file/line, diff hunk, test file, or missing evidence;
- explanation: why this matters functionally;
- suggested next action.

## Functional verdict policy

| Verdict | When to use |
|---|---|
| `functionally_approve` | All relevant acceptance criteria are met or only non-blocking suggestions remain. |
| `comment_only` | There are uncertainties, missing evidence, or minor gaps that need human clarification but do not clearly block merge. |
| `request_changes` | One or more acceptance criteria are not met, docs are violated, or tests/evidence are insufficient for critical behavior. |

## Output contract

The agent should produce Markdown suitable for an MR comment:

1. Short summary.
2. Functional verdict.
3. Acceptance criteria traceability matrix.
4. Documentation checked.
5. Blocking findings.
6. Warnings/suggestions.
7. Open questions.
8. Confidence and limitations.

## Guardrails

- Never invent Jira acceptance criteria or documentation content.
- If access is missing, state exactly what could not be read.
- Do not expose secrets or sensitive data from code, tickets, or logs.
- Prefer precise file/line evidence over broad claims.
- Separate functional blockers from style preferences.
- Flag ambiguity instead of resolving product conflicts silently.
- If project docs and Jira conflict, do not choose a side without saying so.

## Example use cases

### Good fit

- A backend MR claims to implement a pricing rule from Jira.
- A frontend MR changes a checkout flow documented in product docs.
- A migration changes API behavior that has contract documentation.
- A bugfix MR needs regression coverage against a Jira reproduction scenario.

### Poor fit

- Pure dependency update with no product behavior change.
- Formatting-only MR.
- Experimental spike with no acceptance criteria.

## Six-month maintenance notes

When revisiting this agent later, check:

- whether the team uses GitLab MRs or GitHub PRs;
- how Jira acceptance criteria are formatted;
- where project documentation actually lives;
- whether the agent is allowed to post comments automatically;
- whether verdict labels match team workflow;
- whether the traceability matrix is too verbose for real MR comments.

## Related prompt documents

- [`../prompts/code-mr-functional-reviewer/system-prompt.md`](../prompts/code-mr-functional-reviewer/system-prompt.md)
