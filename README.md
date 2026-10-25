# OpportunityLens — Trustworthy Learning Resource Agent

**A grounded, S3-backed learning-resource tool for CareerPath AI**  
*AWS Builder Center Weekend Challenge: Give Your Agent a Tool*

OpportunityLens adds one carefully bounded tool to a career-learning agent: it retrieves resources from a curated JSON catalog in Amazon S3, validates the records, applies deterministic filters, and returns official links with provenance and review-age metadata. If a topic has no catalog match, it returns no match instead of inventing a course or URL.

> **Honest status:** the local interactive demo and offline tests are designed to run without AWS credentials. The live Strands + Amazon Bedrock + Amazon S3 flow still requires a bucket, IAM permissions, model access, and an actual invocation in your AWS account. Do not describe the live path as tested until you run it.

## Why it matters

- **Grounded links:** resource records come from a curated catalog, not open-ended model-generated lists.
- **No fabricated fallback:** unmatched skills return an explicit empty result.
- **Constrained tool scope:** the model cannot choose the S3 bucket or object key; the tool uses read-only S3 GetObject.
- **Validation before use:** schema, unique IDs, object size, field lengths, enum values, dates, HTTPS URLs, and exact approved hostnames are checked.
- **Visible review age:** entries older than 180 days are marked REVIEW_DUE. This is a reminder for human review, not a live URL or cost check.
- **Retrieval provenance:** a live S3 result includes SHA-256 and, where available, ETag, object version, and last-modified metadata.
- **An actual UI:** app.py provides a Streamlit interface with a local demo mode and a separate live-agent mode.
- **Before/after story:** agent_before.py is a text-only baseline; agent.py registers the one new S3-backed tool with Strands Agents.

## Run the interactive app locally

Requires Python 3.10+.

` ` `bash
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
streamlit run app.py
` ` `

Choose **Interactive local demo** to search the bundled JSON catalog without AWS calls. Try:
- data science with budget Free only
- Python with format documentation
- quantum underwater basket weaving to see a deliberate empty result

The CLI is also available:

` ` `bash
python main.py --skill "data science" --budget free
python main.py --skill "Python" --format documentation
python main.py --skill "quantum underwater basket weaving"
python -m unittest discover -s tests -v
python -m compileall -q .
` ` `

## Architecture

` ` `mermaid
flowchart LR
    U[Learner] --> A[CareerPath AI / Strands Agent]
    A --> T[search_verified_learning_resources]
    T --> C[One configured S3 JSON object]
    C --> V[Size, schema, HTTPS and host validation]
    V --> F[Deterministic filters]
    F --> R[Resources, review status and provenance]
` ` `

The app's local mode uses the bundled catalog. The live mode sends a request through Strands Agents on Amazon Bedrock and registers the same search tool; it needs valid AWS credentials or an attached AWS workload role.

## Configure a live S3 + Bedrock run

1. Create a **private** S3 bucket and keep Block Public Access enabled.
2. Upload data/learning_resources.json to catalog/learning_resources.json.
3. Review docs/iam-policy-example.json, replace the bucket placeholder, and grant only s3:GetObject on that object. Do not grant write/delete access to this tool.
4. Configure credentials through an IAM role / AWS SDK credential chain. Prefer a workload role; never commit keys.
5. Set AWS_REGION, LEARNING_CATALOG_BUCKET and LEARNING_CATALOG_KEY=catalog/learning_resources.json. Optionally set LEARNING_CATALOG_EXPECTED_BUCKET_OWNER.
6. Enable a supported Bedrock model in that region and grant the runtime permission to invoke it. Set BEDROCK_MODEL_ID only to override the default selection.
7. Run:

` ` `bash
python agent.py "I am a beginner learning Python for data science. Recommend free documentation resources."
` ` `

Or open app.py, select **Live Strands agent (AWS required)**, and submit a prompt. Confirm that the tool was actually invoked and results match the S3 catalog. AWS calls may incur charges.

## Tests and what they prove

The suite covers positive filters, deterministic empty results, budget/format filters, limits, review-age status, duplicate IDs, invalid URLs, unapproved hosts, invalid dates, schema versions, query validation, mocked S3 reads, SHA-256/version metadata, expected-owner enforcement, missing configuration, oversized objects, and invalid JSON.

GitHub Actions installs dependencies, runs the tests, runs two CLI smoke cases, and compiles Python. A green CI run proves offline/mocked checks passed; it does **not** prove that your AWS account, Bedrock permissions, S3 bucket or hosted app are configured successfully.

## Deployment

See docs/DEPLOYMENT.md. The repository has a Streamlit app and Dockerfile, but a running public application URL is not created merely by pushing source code. For live AWS-backed deployment, use an AWS-hosted service with a narrowly scoped IAM workload role. Never place AWS credentials in public code or browser-side JavaScript.

## Copy-ready AWS Builder Center details

**Title**  
OpportunityLens: Trustworthy Learning Resource Agent

**Description**  
OpportunityLens extends a career-learning agent with one read-only Amazon S3 tool for grounded learning-resource retrieval. The Strands agent searches a curated JSON catalog, validates records and approved HTTPS domains, filters by skill, budget, and format, and returns official links with review-age and S3 provenance metadata. A deliberate no-match result helps avoid fabricated recommendations. It includes a Streamlit demo, a text-only baseline, offline unit tests, mocked-S3 tests, an IAM least-privilege template, and a documented live Bedrock/S3 setup. Review dates are human-maintained signals—not live URL, price, quality, or availability checks.

**Technologies**  
Amazon S3, Amazon Bedrock, Strands Agents SDK, Python, boto3, Streamlit, GitHub Actions.

**Suggested tags**  
Artificial Intelligence; Generative AI; Amazon S3; Amazon Bedrock; Python; Responsible AI.

**Demo field**  
The challenge requires a publicly accessible video under 3 minutes. Follow docs/DEMO_SCRIPT.md, record the actual flow, verify public access in a signed-out browser, and paste the video URL into Builder Center. A repository URL is not a replacement for the required video.

## Repository map

- app.py — Streamlit interface (local demo + live-agent mode)
- agent.py — Strands agent with the new tool
- agent_before.py — text-only baseline
- catalog_tool.py — S3 reader, validation, deterministic search and integrity metadata
- main.py — offline CLI demo
- data/learning_resources.json — sample catalog with official documentation links
- tests/test_catalog_tool.py — unit and mocked-S3 tests
- docs/ARCHITECTURE.md — data flow and boundaries
- docs/DEPLOYMENT.md — deployment guidance
- docs/SECURITY.md — threat assumptions and limitations
- docs/DEMO_SCRIPT.md — judge-facing video walkthrough
- docs/iam-policy-example.json — least-privilege policy template

## Official references

- [Strands Agents Python quickstart](https://strandsagents.com/docs/user-guide/sdk/quickstart/python/)
- [Strands tools guide](https://strandsagents.com/docs/user-guide/sdk/tools/)
- [Amazon S3 GetObject API](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html)
- [Amazon Bedrock getting started](https://docs.aws.amazon.com/bedrock/latest/userguide/getting-started.html)
- [Python Tutorial](https://docs.python.org/3/tutorial/index.html)
- [PostgreSQL Tutorial](https://www.postgresql.org/docs/current/tutorial.html)
- [pandas getting started](https://pandas.pydata.org/docs/getting_started/)
- [scikit-learn User Guide](https://scikit-learn.org/stable/user_guide.html)
