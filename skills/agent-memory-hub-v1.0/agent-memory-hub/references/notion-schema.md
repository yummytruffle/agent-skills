# Notion schema — Agent Memory Hub v1.0

## Root page

Create a top-level page titled `Agent Memory Hub` in a user-selected parent page or workspace location. Keep IDs in host configuration or a clearly labelled root-page configuration section. Never store integration secrets in Notion.

Recommended root sections:
1. Overview and usage
2. Sessions database
3. Shared Memory database
4. Setup/operations notes
5. Optional linked views (may require manual setup depending on API/client)

## Database A: Sessions

One row/page per conversation session.

| Property | Notion type | Required | Allowed values / meaning |
|---|---|---:|---|
| Name | title | yes | Human-readable session title |
| Session ID | rich_text | yes | Stable unique ID from source platform; otherwise persisted UUID |
| Started At | date | yes when known | Session start time; do not invent |
| Last Synced At | date | yes after write | Timestamp of latest successful sync |
| Agent | rich_text | yes when known | Source agent/client |
| Project | select | no | Project name; use controlled values where practical |
| Topics | multi_select | no | Topic tags |
| Session Type | select | yes | Research, Coding, Planning, Decision, Other |
| Status | select | yes | Active, Completed, Archived, Error |
| Summary | rich_text | yes | Short summary (keep within API/property limits; full summary is in body) |
| Memory Records | relation -> Shared Memory | yes after schema ready | Related memory pages |
| Source URL | url | no | Original conversation URL |
| Transcript Status | select | yes | Complete, Partial, Link Only, Unavailable |

`Ended At` may be added later if useful. `Started At` and source timestamps must reflect actual source data, not save time.

## Database B: Shared Memory

One row/page per durable memory unit. The detailed content lives in page body, not in an oversized property.

| Property | Notion type | Required | Allowed values / meaning |
|---|---|---:|---|
| Name | title | yes | Self-contained, conclusion-style title |
| Memory ID | rich_text | yes | Stable unique ID |
| Memory Type | select | yes | Preference, Fact, Decision, Procedure, Insight, Goal, Task |
| Project | select | no | Project name |
| Tags | multi_select | no | Search tags |
| Source Sessions | relation -> Sessions | yes | Provenance relation |
| Confidence | select | yes | Confirmed, Inferred, Unverified |
| Status | select | yes | Active, Superseded, Expired, Needs Review |
| Created At | date | yes | First created |
| Updated At | date | yes | Last meaningful update |
| Valid Until | date | no | Optional expiry |
| Sensitivity | select | yes | Normal, Sensitive, Restricted |
| Supersedes | relation -> Shared Memory | no | Old memory replaced by this memory |

### Relation setup

Create both databases first, then add reciprocal relations:
- Sessions.`Memory Records` -> Shared Memory
- Shared Memory.`Source Sessions` -> Sessions

Notion may expose reciprocal relation configuration differently depending on API version and MCP tool. Verify the resulting schema rather than assuming the second relation was created automatically. If reciprocal relations are unsupported in the available tool, retain at least `Source Sessions` and store the Session page URL/ID in the memory body as a fallback.

### Select options

Create these options during initialization where the API allows it:
- Session Type: `Research`, `Coding`, `Planning`, `Decision`, `Other`
- Session Status: `Active`, `Completed`, `Archived`, `Error`
- Transcript Status: `Complete`, `Partial`, `Link Only`, `Unavailable`
- Memory Type: `Preference`, `Fact`, `Decision`, `Procedure`, `Insight`, `Goal`, `Task`
- Confidence: `Confirmed`, `Inferred`, `Unverified`
- Memory Status: `Active`, `Superseded`, `Expired`, `Needs Review`
- Sensitivity: `Normal`, `Sensitive`, `Restricted`

### Notion API/MCP notes

- Use the schema actually returned by the connected Notion tool. Notion API versions and MCP wrappers may differ in whether they call these objects databases or data sources.
- Notion rich-text properties have size limits; the full summary/transcript belongs in page blocks.
- Use pagination for queries and block children.
- Use batched block appends for long content.
- A linked database view inside each Session page is a presentation feature and may not be creatable by the exposed API/MCP. The reliable v1 fallback is the `Memory Records` relation property plus links to related memory pages. Create a filtered linked view manually if desired.
- Do not assume a single database query provides semantic search. Keyword filters are the baseline; semantic/vector search is a future extension.
