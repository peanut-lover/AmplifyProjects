# Amplify Project

Mono Repo For training Amplify CLI

# Mono Repository

- First, Check NPM Version supporting [NPM workspace](https://docs.npmjs.com/cli/v7/using-npm/workspaces)

Use NX [`package-based` scaffold](https://nx.dev/getting-started/package-based-repo-tutorial)

```bash
$ npx create-nx-workspace@latest package-based --preset=npm
```

# Training Branches

- Serverless Lambda and API Gateway (branch: `training/serverless`)
- Auth(AWS Cognito Service) with custom form (branch: `training/auth`)
- GraphQL (AWS AppSync) (branch: `training/graphql`)
- Other Lambda Triggers(PostConfirmation, S3 Uplaod) (branch: `training/lambda-s3-event-trigger`)
