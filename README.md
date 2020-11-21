# AWS CloudFormation Starter Workflow for GitHub Actions

This template repository contains a sample application and sample GitHub Actions workflow files for continuously deploying both application code and infrastructure as code with GitHub Actions.

## Create a GitHub repository from this template

Deploy the IAM resources needed to enable GitHub Actions to deploy CloudFormation templates:

```
aws cloudformation deploy \
  --stack-name github-actions-cloudformation-deploy-setup \
  --template-file stack.yml \
  --capabilities CAPABILITY_NAMED_IAM \
  --region eu-west-1
```

You can review the permissions that your repository's GitHub Actions deployment workflow will have in the [stack.yml](stack.yml) CloudFormation template.

Retrieve the IAM access key credentials that GitHub Actions will use for deployments:

```
aws secretsmanager get-secret-value \
  --secret-id github-actions-cloudformation-deploy \
  --region eu-west-1 \
  --query SecretString \
  --output text
```

Create two GitHub Actions secrets for the access key in your GitHub repository by going to Settings > Secrets. Alternatively, you can create these GitHub Actions secrets at the GitHub organization level, and grant access to the secrets to your new repository.

1. Create a secret named `AWS_ACCESS_KEY_ID` containing the `AccessKeyId` value returned above.
1. Create a secret named `AWS_SECRET_ACCESS_KEY` containing in the `SecretAccessKey` value returned above.

Go to the Actions tab, select the latest workflow run and its failed job, then select "Re-run jobs" > "Re-run all jobs".

When the workflow successfully completes, expand the "Print service URL" step in the "Deploy web application" job to see the URL for the deployed web application.
