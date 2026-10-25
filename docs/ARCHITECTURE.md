# Architecture and design decisions

## Request flow
1. A learner asks CareerPath AI for resources.
2. A Strands agent on Amazon Bedrock receives instructions to use the catalog tool for named resources, links, formats, and cost claims.
3. `search_verified_learning_resources` reads one preconfigured S3 object; bucket and key are environment settings, not model-controlled arguments.
4. The tool rejects oversized or malformed payloads and validates schema, IDs, approved HTTPS hosts, fields, enumerations, and dates.
5. Deterministic filters select matches by skill terms, cost preference, and format.
6. Entries include a curator review date and age status. A live S3 read includes SHA-256 and optional version, ETag, and last-modified metadata.
7. The agent explains matches without inventing substitutes when the catalog has no match.

## Trust boundaries
- **Model to tool:** inputs are validated; the model cannot choose arbitrary bucket/key values.
- **S3 to parser:** payload is capped at 2 MiB; UTF-8 JSON and schema/host/field checks run before use.
- **Catalog to UI:** catalog values are displayed as text and links, not executed HTML.
- **Runtime to AWS:** S3 read-only access and Bedrock invocation are separate permissions. Prefer a workload role.
- **Offline vs. live:** local mode uses bundled JSON. Live mode contacts Bedrock and can invoke the S3 tool.

## Trade-offs
A small curated catalog is safer and more inspectable than open-web crawling but needs human maintenance. Deterministic matching is transparent but less semantically broad than vector search. Review age is a reminder—not proof of live URL response, cost, quality, or availability.
