# 2 minute 30 second judge-facing demo

Record the actual behavior of your deployed setup. The local demo and live AWS demo are different modes—label them accurately.

## 0:00–0:20 — Problem and impact
Introduce CareerPath AI's learning-resource stage. Explain the risk: a model relying only on memory may produce plausible but ungrounded resource links or cost details. OpportunityLens adds one retrieval tool to ground named resources in a maintained catalog.

## 0:20–0:50 — Show the app UI
Open Streamlit in Interactive local demo mode. Search data science with Free only. Show official links, IDs, cost labels, curator review dates, and result count. State clearly that this uses bundled sample data, not AWS.

## 0:50–1:15 — Show deliberate no-match behavior
Search quantum underwater basket weaving. Show that no substitute link appears; the system does not invent a recommendation.

## 1:15–1:45 — Explain the one new tool
Show `catalog_tool.py` and `agent.py`. Highlight bounded S3 read, validation, exact host allowlist, deterministic filters, and integrity metadata. The model cannot select arbitrary objects or write/delete catalog data.

## 1:45–2:15 — Live run (AWS setup required)
Switch to Live Strands agent, submit a prompt, and show the actual Bedrock request and tool invocation. Confirm output matches the S3 catalog. If live setup is not complete, do not pretend local mode is live—label integration as pending.

## 2:15–2:30 — Limits and close
State that curator review dates do not prove current URL availability, price, quality, or access. Emphasize read-only scope and human-reviewed maintenance.

Before submission, upload the recording publicly, open it signed out, verify it is under 3 minutes, and paste the URL into Builder Center's demo field. Do not show secrets or personal data.
