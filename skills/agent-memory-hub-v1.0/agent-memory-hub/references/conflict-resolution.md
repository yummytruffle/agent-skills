# Deduplication and conflict resolution

## Decision table

| Situation | Action |
|---|---|
| Exact same Session ID exists | Update that session page; never create another |
| Same memory concept and same conclusion | Update metadata/content if needed; add source relation |
| New evidence reinforces existing memory | Keep one active memory and append provenance/update history |
| Explicit new decision replaces old decision | Create/update the new memory, mark old `Superseded`, set `Supersedes` relation when supported |
| Old fact has a clear expiry | Mark `Expired` after expiry only if expiry is reliable |
| Evidence conflicts and chronology resolves it | Update current memory and preserve history |
| Evidence conflicts but cannot be resolved | Mark `Needs Review`; do not silently overwrite |
| Candidate is low-value or transient | Skip |
| Secret/credential detected | Redact from transcript and do not store as memory |

## Safe update sequence

1. Query by stable ID where available.
2. If no stable-ID match, search for plausible semantic duplicates.
3. Retrieve candidate pages and compare meaning, source, confidence, project, and status.
4. Choose create/update/skip/review.
5. Preserve Source Sessions and append a concise update-history entry.
6. Verify status and relations after write.

## Important distinctions

- A newer statement is not always a replacement; it may apply to a different project or context.
- A brainstorming proposal is not a confirmed decision.
- User correction has priority over an unsupported assistant claim.
- Do not delete historical decisions by default.
- Do not infer an expiry date unless one was stated or follows from a reliable explicit validity rule.
