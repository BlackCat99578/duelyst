# Staging Infrastructure

This directory contains Terraform to spin up a staging environment in AWS.

To get started, first follow the instructions in the general [Terraform README](../README.md).

Then populate `terraform.tfvars` with the keys and values from `variables.tf` and run `./terraform_init.sh`. This
is a helper script which automatically references the `config.aws.tfbackend` and `config.s3.tfbackend` files we created
previously.

Passwords should not be stored in Terraform. Instead, the following keys must be created in AWS
SSM Parameter Store (see `scripts/create-ssm-secret.sh`):

```
/duelyst/staging/postgres/password
```

Finally, run `terraform apply` to see the plan output and provision a staging environment.

## Uploading static assets to S3

1. Build everything with `FIREBASE_URL=foo yarn build`
2. Copy CDN-flagged resources into `dist/` with `yarn cdn:copy`
3. Upload resources to S3:

```
AWS_ACCESS_KEY=foo \
AWS_SECRET_KEY=bar \
AWS_REGION=baz \
S3_ASSETS_BUCKET=your-bucket-name \
yarn cdn:upload:staging
```