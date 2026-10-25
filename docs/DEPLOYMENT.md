# Deployment guide

## Option A — run locally
```bash
python -m venv .venv
python -m pip install -r requirements.txt
streamlit run app.py
```
The default Interactive local demo uses bundled JSON and does not require AWS credentials.

## Option B — AWS-hosted container for live mode
The Dockerfile can be used to build a container and deploy it to AWS App Runner or another managed container service. A typical process is to build the image, push it to Amazon ECR, deploy the image, and attach an IAM workload role.

Before deployment:
1. Create a private S3 bucket and upload the catalog to `catalog/learning_resources.json`.
2. Grant the runtime role `s3:GetObject` for that object ARN only, and separately grant the needed Bedrock invocation permission.
3. Keep S3 Block Public Access enabled.
4. Set `AWS_REGION`, `LEARNING_CATALOG_BUCKET`, `LEARNING_CATALOG_KEY=catalog/learning_resources.json`, and optionally `LEARNING_CATALOG_EXPECTED_BUCKET_OWNER` and `BEDROCK_MODEL_ID`.
5. Configure inbound access, logs, budget alerts, and health monitoring.
6. Verify a live tool invocation, a no-match prompt, and a permission-error case.

Exact steps vary by AWS service and account. A successful image build is not proof of live integration.

## Option C — Streamlit Community Cloud for a UI preview
A public repository can be deployed by selecting `app.py` in Streamlit Community Cloud. The local demo should work without AWS. Streamlit Cloud does **not** automatically inherit an IAM role from your AWS account. Do not paste long-lived keys into public code. For live S3/Bedrock, prefer an AWS-hosted runtime with a workload role or a properly secured backend.

## Link status
The GitHub repository is the source-code link. A running public URL exists only after successful deployment. Do not advertise an unverified URL. After deployment, open the public URL while signed out and confirm the app loads before sharing it.
