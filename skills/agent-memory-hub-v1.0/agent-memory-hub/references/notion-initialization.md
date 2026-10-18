# Fresh Notion initialization plan

## Preconditions

- An authenticated Notion integration/MCP is available to the host.
- The integration has access to the chosen parent page/workspace and permissions to create/read/update pages, databases/data sources, properties, relations, and block children as required.
- User has selected or authorized the parent location.
- Store integration tokens only in the host's secret manager/environment, never in Notion or source control.

## Initialization algorithm (idempotent)

1. Search for a root page named `Agent Memory Hub` in the authorized workspace.
2. If found, inspect its child pages/databases and reuse matching objects. If not found, create the root page under the user-authorized parent.
3. Create or reuse the `Sessions` database under the root.
4. Create or reuse the `Shared Memory` database under the root.
5. Create properties and select options according to `notion-schema.md`.
6. Add reciprocal relation properties. Verify their target IDs and reciprocal configuration.
7. Add a root-page overview with database links, operating rules, and the configured IDs.
8. Verify by retrieving both schemas and querying each database (empty result is acceptable).
9. Return the root URL, database URLs/IDs, schema verification results, and any manual steps.

## API operation mapping

The exact endpoints vary by Notion API version. Map the available API/MCP tools to these logical operations:
- Search pages/data sources.
- Create root page.
- Create database/data source with properties.
- Update database/data source properties to add relations/select options if supported.
- Retrieve database/data source schema.
- Query database/data source.
- Create/update page and append block children.

For direct REST API implementations, consult the current official Notion API documentation and pin the API version header. Do not copy endpoint shapes from an unverified example. Some MCPs expose higher-level operations instead of raw REST calls.

## Linked view limitation

Do not make linked database views a prerequisite for successful initialization. Some APIs/MCPs do not expose a create-linked-view operation. The minimum viable setup is:
- Sessions relation to Shared Memory
- Shared Memory relation to Sessions
- links to each related memory page

If the host cannot create linked views, instruct the user to open a Session page in Notion and insert a linked view of `Shared Memory`, filtered so `Source Sessions` contains the current Session. This is an optional UX enhancement, not a data-integrity requirement.

## Rollback and partial failures

- Before retrying a timed-out create, search for the object by name and inspect children to avoid duplicates.
- If one database succeeds and the second fails, preserve the successful object and resume.
- Do not delete user-created pages/databases during rollback.
- Report partial completion and exact missing steps.
