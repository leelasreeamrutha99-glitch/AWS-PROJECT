# Test status and verification boundaries

## Automated checks
GitHub Actions runs on pushes to main/master and pull requests. It installs dependencies, runs the unit and mocked-S3 suite, executes a local catalog smoke demo and an empty-result scenario, and compiles Python files.

Tests cover deterministic matching, no-match behavior, budget/format filters, result limits, stale review marking, duplicate IDs, URL and trusted-host checks, invalid dates, schema version, query validation, mocked S3 reads, SHA-256 and object metadata, expected-owner enforcement, missing configuration, oversized objects, and invalid JSON.

## What an offline pass proves
A green CI run verifies automated local/mocked checks on the runner's dependency environment. It does **not** prove that the live S3 bucket, IAM permissions, Bedrock model access, Streamlit hosting, or workload role work in a user's AWS account.

## Live verification checklist
1. Upload the catalog to a private S3 bucket.
2. Configure narrowly scoped S3 GetObject and Bedrock invoke permissions.
3. Run `python agent.py "I am a beginner learning Python for data science. Recommend free documentation resources."`.
4. Confirm the actual tool invocation and match resource IDs/URLs to the S3 object.
5. Test an unmatched query, a review-due record, and an S3 permission failure.
6. Record a public video under 3 minutes showing the live call without exposing credentials.

**Live AWS account testing is not claimed until the checklist above has actually been completed.**
