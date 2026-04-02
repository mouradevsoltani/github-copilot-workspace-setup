# github-copilot-workspace-setup

Configuration workspace for custom Copilot agents, instructions, and reusable skills.

## Purpose

This repository provides three layers:

- `agents/`: role-specific agent behaviors
- `instructions/`: automatically applied contextual rules
- `skills/`: reusable technical playbooks and reference hubs

Use this setup to keep agent behavior stable, make technical guidance reusable, and reduce prompt duplication.

## Structure

```text
.github/
├── agents/
├── instructions/
├── skills/
└── README.md
```

## Precedence

Apply guidance in this order:

1. Project-local instructions in the target repository, for example `.github/instructions/*`
2. `.github/instructions/*`
3. `.github/skills/*`
4. Existing repository conventions inferred from code

Behavior rules belong in `instructions/`.
Technical implementation guidance belongs in `skills/`.

## Agents

| Agent | Role |
|---|---|
| `supervisor` | Scope requests, frame approach, assign work |
| `developer` | Implement backend, frontend, and DevOps changes |
| `code-reviewer` | Review correctness, maintainability, and merge readiness |
| `security-auditor` | Audit vulnerabilities, misconfigurations, and supply-chain risk |
| `qa-tester` | Design and generate tests |
| `doc-writer` | Produce and maintain project documentation |

Agent definitions live in `agents/` and should stay focused on behavior, not framework detail.

## Recommended Workflow

Use the following default workflow for most requests.

### 1. Intake and classification

- Start with `supervisor` when the task is ambiguous, cross-cutting, or likely to touch multiple layers.
- Let `supervisor` classify the request as:
	- small change
	- feature
	- cross-cutting change
	- architectural change
- For very small and obvious implementation tasks, `developer` can start directly.

### 2. Context loading

- Apply repository-local instructions first when they exist.
- Then load relevant `.github/instructions/*`.
- Then use the relevant skill hub and its references.
- For Symfony work, use `instructions/symfony.instructions.md` for behavior and `skills/php-symfony/SKILL.md` for technical rules.

### 3. Implementation

- `developer` implements the requested change.
- `doc-writer` is used instead of `developer` when the task is documentation-only.
- `developer` should keep scope tight, follow repository conventions, and leave handoff notes for QA, review, and security when relevant.

### 4. Review and hardening

- `code-reviewer` checks correctness, maintainability, consistency, and missing tests.
- `qa-tester` adds or evaluates test coverage for critical and changed paths.
- `security-auditor` reviews authentication, authorization, secrets, unsafe input flows, deployment risk, and supply-chain concerns.

### 5. Documentation and delivery

- `doc-writer` updates operational, architectural, or user-facing documentation if the change affects behavior or workflows.
- `supervisor` can be used again at the end for cross-agent synthesis, open risks, and completion criteria.

## Workflow by request type

| Request type | Recommended flow |
|---|---|
| Small implementation | `developer` -> `code-reviewer` -> `qa-tester` if needed |
| Backend/frontend feature | `supervisor` -> `developer` -> `code-reviewer` -> `qa-tester` -> `doc-writer` if needed |
| Security-sensitive change | `supervisor` -> `developer` -> `security-auditor` -> `code-reviewer` -> `qa-tester` |
| Documentation task | `doc-writer` |
| Code review only | `code-reviewer` |
| Test generation only | `qa-tester` |
| Architecture framing | `supervisor` |

## Delivery Standard

The preferred delivery flow is:

1. Classify and scope the request
2. Load the right instructions and skill references
3. Implement only the requested change
4. Review correctness and risks
5. Validate tests and security where needed
6. Update documentation if behavior changed
7. Deliver a concise final summary with remaining risks or next steps

## Instructions

Instructions are automatically applied by file pattern or usage context.

Current notable instruction groups:

- `symfony.instructions.md`: Symfony agent behavior only
- `security-and-owasp.instructions.md`: secure-by-default baseline
- `code-review-generic.instructions.md`: review-specific guidance
- `memory-bank.instructions.md`: lean memory workflow

Keep instructions short, stable, and non-duplicative.
If an instruction starts carrying too much technical detail, move that detail into a skill reference.

## Skills

Skills are reusable technical guides.

Notable skill hubs:

- `php-symfony/`: Symfony technical hub plus focused references
- `quality-playbook/`: end-to-end quality system generation
- `security-review/`: security-focused implementation guidance
- `webapp-testing/`: browser and UI validation workflow
- `refactor/`, `refactor-plan/`: refactoring tactics and sequencing

The preferred pattern is:

1. One concise `SKILL.md` as the entry point
2. Detailed technical content split into `references/*.md`
3. Minimal duplication between the hub and the references

Use a skill when the task needs technical depth, repeatable implementation patterns, or specialized references.

## Quick Navigation

Use this section to find the right entry quickly.

| Need | Start here |
|---|---|
| Plan or classify a request | `agents/supervisor.agent.md` |
| Implement a feature or fix | `agents/developer.agent.md` |
| Review a change | `agents/code-reviewer.agent.md` |
| Audit security risk | `agents/security-auditor.agent.md` |
| Generate or assess tests | `agents/qa-tester.agent.md` |
| Write or reorganize documentation | `agents/documentation-writer.agent.md` |
| Symfony technical guidance | `skills/php-symfony/SKILL.md` |
| Quality system / review protocols | `skills/quality-playbook/SKILL.md` |
| Browser or UI validation | `skills/webapp-testing/SKILL.md` |
| Security implementation patterns | `skills/security-review/SKILL.md` |

## First Read, Then Read

Recommended reading order for any new task:

1. Read this `README.md`
2. Identify the right agent for the task
3. Apply repository-local instructions if they exist
4. Apply relevant `.github/instructions/*`
5. Open the relevant skill hub in `.github/skills/*`
6. Read only the skill references needed for the current task

## Agent Routing Matrix

| Situation | Primary agent | Supporting assets |
|---|---|---|
| Ambiguous or multi-step request | `supervisor` | relevant instructions + relevant skills |
| Direct code change | `developer` | language/framework instructions + skill hub |
| Merge-readiness check | `code-reviewer` | `code-review-generic.instructions.md` |
| Sensitive auth/input/secrets change | `security-auditor` | `security-and-owasp.instructions.md`, `security-review/` |
| Missing tests or unstable coverage | `qa-tester` | test-related skills |
| Documentation or workflow guide | `doc-writer` | markdown/doc structure guidance |