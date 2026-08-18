---
document_type: deployment
authority_class: descriptive
status: draft
---

# Deployment

## Prerequisites

List the software, accounts, credentials, permissions, and configuration required before deployment.

## Deployment Steps

1. Prepare the local environment.
2. Review and configure project variables.
3. Initialize Terraform.
4. Review the execution plan.
5. Deploy the infrastructure.

## Terraform Commands

```bash
terraform init
terraform fmt -check
terraform validate
terraform plan
terraform apply
```

## Cleanup

Destroy disposable lab resources when validation is complete:

```bash
terraform destroy
```

## Notes

Document deployment observations, troubleshooting steps, timing considerations, or anything another engineer should know when reproducing the deployment.
