# Memory extraction policy

## Candidate scoring

Consider a memory candidate when one or more are true:
- The user explicitly says to remember, reuse, or treat something as a preference.
- It is a confirmed project fact, constraint, architecture decision, or durable goal.
- It explains why an important decision was made.
- It is a reusable procedure or lesson likely to help another session/agent.
- It corrects a previously stored memory.

Do not store:
- Greetings, filler, routine small talk, or repeated paraphrases.
- Unconfirmed assistant speculation as fact.
- One-off temporary details without future utility.
- Passwords, API keys, access tokens, private keys, recovery codes, or secrets.
- Sensitive personal information unless it is necessary and explicitly authorized.

## Required fields in memory body

## Content
A concise, standalone statement.

## Context
Why it matters and where it applies.

## Rationale / Evidence
Reasoning or evidence; distinguish user-provided facts from assistant analysis.

## Applicability
Project, environment, conditions, and known exceptions.

## Limitations
Unknowns, caveats, or verification needed.

## Source Evidence
Source Session link/ID and, where possible, message timestamp or transcript section.

## Update History
Timestamp — change — reason. Add an entry for meaningful updates.

## Confidence semantics
- Confirmed: explicitly stated by the user or supported by direct evidence.
- Inferred: plausible interpretation; must be labelled as inference.
- Unverified: candidate claim requiring confirmation.

## Sensitivity
- Normal: ordinary non-sensitive project/work knowledge.
- Sensitive: personal or confidential information; minimize and follow explicit authorization.
- Restricted: secrets/credentials are not stored. Use this label only as a rejection/review marker, not permission to save secrets.

## Memory ID and deduplication
Generate a stable UUID for new records. Before creating, compare title, type, project, tags, content, and source. If the same concept already exists, update or add provenance rather than create a duplicate. Do not rely only on title equality.
