# Lab 01 — Least Privilege IAM Policy

## Scenario

A Lambda function needs to read objects from a single S3 bucket called `my-app-bucket`. Nothing else.

## What NOT to do

```json
{
  "Effect": "Allow",
  "Action": "s3:*",
  "Resource": "*"
}
```

This gives full S3 access across every bucket — a massive blast radius if the role is ever compromised.

## What to do instead

See [`policy.json`](./policy.json) — it scopes down to:
- Only `s3:GetObject` and `s3:ListBucket`
- Only the specific bucket ARN

## Why it matters

If this role is ever exfiltrated or misused, the attacker can only read one bucket. They can't delete, write, or touch anything else in S3 — let alone other services.

## Testing the policy (AWS CLI)

```bash
aws iam simulate-principal-policy \
  --policy-source-arn arn:aws:iam::123456789012:role/MyLambdaRole \
  --action-names s3:GetObject \
  --resource-arns arn:aws:s3:::my-app-bucket/test.txt
```