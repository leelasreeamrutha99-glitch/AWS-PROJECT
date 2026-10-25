# Integrating OpportunityLens with CareerPath AI

The existing CareerPath AI concept includes a Learning Resource & Opportunity Navigator stage. OpportunityLens adds grounded retrieval for named learning resources from a curated JSON catalog in Amazon S3.

Suggested instruction for that stage:

> When recommending named learning resources, official URLs, formats, or cost labels, call `search_verified_learning_resources` first. Use only returned catalog entries for specific resource claims. Preserve resource IDs and official URLs. Do not infer prices from missing data. If no result matches, say the catalog has no match. If `review_status` is `REVIEW_DUE`, tell the learner to recheck the official page.

The tool is read-only and cannot create, update, or delete catalog objects. Catalog changes should follow a reviewed maintenance process. The bundled local demo demonstrates deterministic retrieval; it is not live S3 access.
