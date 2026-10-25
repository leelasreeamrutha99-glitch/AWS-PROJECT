# Security model and limitations

## Implemented controls
- One environment-configured S3 bucket/object; the model cannot pass bucket/key values.
- Read-only S3 `GetObject`; no catalog write/delete tool.
- A 2 MiB payload cap and bounded resource count.
- UTF-8 JSON parsing and schema-version validation.
- Unique constrained resource IDs; bounded fields; enumerated levels and formats.
- Absolute HTTPS URLs, embedded URL credentials rejected, exact match against a small official-host allowlist.
- Review dates cannot be in the future; entries older than 180 days are marked `REVIEW_DUE`.
- The system prompt treats catalog contents as data rather than instructions and prohibits invented fallbacks.
- SHA-256 fingerprints retrieved bytes. It does not prove authorship or content freshness.

## Deployment requirements
Keep the catalog bucket private and S3 Block Public Access enabled. Prefer an IAM role; grant access to the exact catalog object and scope Bedrock permissions separately. Never commit AWS keys, tokens, or local AWS profiles. Avoid exposing secrets or sensitive learner data in logs or demo videos. Consider S3 versioning, CloudTrail, budget alerts, and human-reviewed catalog publishing.

## Explicit limitations
The app does not make live HTTP requests to check URLs. It does not confirm current prices, access, accreditation, quality, or learning outcomes. The sample catalog is a small starter dataset, not comprehensive course coverage. Trusted hosts constrain destinations but do not guarantee page content remains correct. Live AWS functionality must be tested in the deployment account; offline tests are not a substitute.
