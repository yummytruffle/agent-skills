# Agent Memory Hub v1.0

A cross-platform Skill specification for Notion-backed shared memory.

## Decisions
- One global Sessions database; one global Shared Memory database.
- One Session = one Sessions row/page.
- Every Session page contains a structured summary and full available transcript.
- Durable memory items are separate pages in Shared Memory and relate to source sessions.
- Save automatically only when the host actually invokes the skill at session end.
- Retrieval is explicit; do not automatically read memory at conversation start.
- Initialization is fresh and idempotent; only new sessions are in scope for v1.0.

## Files
- `SKILL.md`: core instructions and abstract tool contract.
- `references/notion-schema.md`: properties and relation model.
- `references/session-template.md`: session body structure.
- `references/memory-extraction.md`: candidate rules and confidence/sensitivity.
- `references/conflict-resolution.md`: dedupe and updates.
- `references/notion-initialization.md`: setup algorithm and API limitations.
- `tests/acceptance-tests.md`: acceptance checklist.

## Important
This package defines behavior; it does not itself connect to Notion or create databases. A host must provide authenticated Notion API/MCP operations. Session-end auto-capture also requires a host trigger or automation. Cross-platform means the instructions are portable; each host may need a small adapter mapping abstract operations to its actual Notion tools.
