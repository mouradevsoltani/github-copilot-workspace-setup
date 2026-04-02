---
applyTo: '**'
description: 'Lean memory bank workflow for persistent context across sessions.'
---

# Memory Bank (Lean)

Use memory as a lightweight continuity tool, not as a full project-management system.

## Scopes
- User memory: /memories/
- Session memory: /memories/session/
- Repository memory: /memories/repo/

## Core Rules
- Before creating a memory file, inspect /memories/ to avoid duplicates.
- Keep entries short and factual (1-5 bullets).
- Store only durable value: user preferences, stable repo conventions, recurring pitfalls.
- Do not store transient logs or verbose narratives.
- Update or delete stale facts when proven wrong.

## When To Write Memory
- New stable convention was discovered.
- A repeated mistake was identified with a clear prevention rule.
- The user stated an explicit preference that should persist.

## When Not To Write Memory
- One-off task details with no reuse value.
- Temporary troubleshooting output.
- Information already obvious from repository files.

## Suggested Format
- Context: one line.
- Decision: one line.
- Rule/Preference: bullet list.
- Last verified: YYYY-MM-DD.

## Quality Bar
Each memory entry should answer: "Will this help future sessions make better decisions quickly?"
If not, do not store it.
