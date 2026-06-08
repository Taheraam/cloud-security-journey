# 01 — IAM: Identity & Access Management

## What is IAM?

IAM (Identity and Access Management) is the system that controls **who can do what** in a cloud environment. In AWS, every action — reading an S3 bucket, launching an EC2 instance, calling a Lambda — is an API call, and IAM decides whether that call is allowed or denied.

## Core Concepts

### Principals
The entity making a request. Can be:
- **IAM User** — a person or application with long-term credentials
- **IAM Role** — a temporary identity assumed by services, apps, or users
- **AWS Service** — e.g. Lambda, EC2 assuming a role to act on your behalf

### Policies
JSON documents that define permissions. Two main types:
- **Identity-based policies** — attached to users, groups, or roles
- **Resource-based policies** — attached directly to resources (e.g. S3 bucket policy)

### The Evaluation Logic
When a request comes in, AWS evaluates in this order:
1. Explicit **Deny** → always wins
2. Explicit **Allow** → permitted
3. Default → **Deny** (nothing is allowed unless explicitly permitted)

### Least Privilege
The golden rule of IAM: grant only the permissions needed to do the job, nothing more. A Lambda that only reads from one S3 bucket should have exactly that — not `s3:*`.

## Common Mistakes

- Using root account credentials for anything
- Attaching `AdministratorAccess` to application roles
- Never rotating access keys
- Wildcard actions (`*`) on sensitive resources
- No MFA on privileged accounts

## Key AWS IAM Resources

| Resource | Purpose |
|----------|---------|
| `aws iam list-users` | List all IAM users |
| `aws iam list-attached-role-policies` | See what's attached to a role |
| `aws iam simulate-principal-policy` | Test what a principal can do |
| `aws iam get-account-authorization-details` | Full account IAM snapshot |

## Lab

→ [Least Privilege Policy Demo](./labs/01-least-privilege-demo/walkthrough.md)