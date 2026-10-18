# Acceptance tests — Agent Memory Hub v1.0

Run in a test workspace or under a dedicated test parent page. Use synthetic data only.

## A. Initialization

1. Run `initialize` in an empty test parent.
   - Expect one root page, one Sessions database, one Shared Memory database.
   - Expect schemas and select options to match `references/notion-schema.md`.
   - Expect reciprocal relations or a clearly reported fallback.
2. Run `initialize` again.
   - Expect no duplicate root page or databases.
3. Simulate a failure after creating Sessions but before Shared Memory.
   - Retry.
   - Expect Sessions reused and Shared Memory created once.
4. Disable database creation permission.
   - Expect a clear permission error and no false success claim.

## B. Session saving

5. Save a session with summary and transcript.
   - Expect one Sessions row/page, summary sections, transcript, timestamps when available, and transcript status `Complete`.
6. Save the same session twice with the same Session ID.
   - Expect one row/page; existing page updated; no duplicate transcript blocks.
7. Save when only partial transcript is available.
   - Expect `Transcript Status=Partial` and an explicit limitation note.
8. Save a very long transcript.
   - Expect batched writes, preserved message order, and a verifiable partial/resume strategy.
9. Include a fake API key in synthetic transcript input.
   - Expect secret redacted from archive and no memory created from it.

## C. Memory extraction and relations

10. Include a confirmed decision, a durable preference, a transient remark, and an assistant guess.
    - Expect durable candidates only; correct type and confidence; transient remark skipped; guess not marked confirmed.
11. Save a repeated decision from a second session.
    - Expect existing memory updated or provenance appended, not a duplicate concept.
12. Save an explicitly superseding decision.
    - Expect new active record, old record `Superseded`, and relation where supported.
13. Save conflicting evidence with no clear resolution.
    - Expect `Needs Review`, not silent overwrite.
14. Verify each memory links to its source Session and Session links to related memories when relation updates are supported.

## D. Retrieval policy

15. Start a normal new session without asking for memory.
    - Expect no historical memory query.
16. Explicitly ask “search memory for the database decision”.
    - Expect query, relevant results, status/confidence, and source links.
17. Search a nonexistent topic.
    - Expect a clear no-results answer; no invented memory.
18. Query a superseded decision for historical context.
    - Expect it labelled superseded and the replacement identified if available.

## E. Reliability and reporting

19. Simulate a timeout after a create request.
    - On retry, search before creating; no duplicate object.
20. Simulate a relation write failure.
    - Expect session/memory write status reported accurately and relation retried or flagged.
21. Remove transcript-read capability from host.
    - Expect archive summary if possible, transcript status `Unavailable` or `Partial`, and no claim that full transcript was saved.
22. Verify completion response includes Session URL, transcript status, memory create/update/skip/review counts, and any failures.

## Release gate

Do not call v1.0 production-ready until tests 1–3, 5–7, 10–14, 15–17, and 19–22 pass in the intended host environment.
