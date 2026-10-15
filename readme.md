# HelloWorld

This repository contains a minimal Python FastAPI backend (Hello World) and a placeholder frontend to be hosted on S3 + CloudFront. The backend is Dockerized and intended to be deployed to AWS ECS Fargate behind an ALB. GitHub Actions provide CI and CD workflows.

Repository layout
- app/main.py - FastAPI app
- requirements.txt
- Dockerfile
- ecs/task-def.json.template - ECS task definition template (replace placeholders)
- frontend/index.html - placeholder static site to sync to S3
- .github/workflows/ci.yml - CI workflow (lint & tests)
- .github/workflows/cd.yml - CD workflow (build/push to ECR, register task, update ECS service, sync S3, invalidate CloudFront)

Required repository secrets (Settings → Secrets and variables → Actions)
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY
- AWS_REGION (e.g. us-east-1)
- ECR_REPOSITORY (e.g. hello-python-backend)
- ECS_CLUSTER
- ECS_SERVICE
- S3_BUCKET
- CLOUDFRONT_DISTRIBUTION_ID

Security recommendations
- Prefer GitHub OIDC (workflows can assume an AWS role) instead of long-lived AWS credentials.
- Restrict IAM permissions to the specific resources (ECR, ECS, S3, CloudFront) and use least privilege.
- Use GitHub Environments for production deployments and require reviewers/approvals.

Next steps
1. Add the repository secrets listed above.
2. Create the ECR repository, ECS cluster/service, S3 bucket, and CloudFront distribution (or provide their names/IDs in the secrets).
3. Open a PR to trigger CI; merge to main to trigger the CD pipeline.

If you want, I can also generate Terraform to create the AWS resources and/or convert the CD workflow to use OIDC assume-role. Let me know which you prefer.
