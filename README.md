# actions-apollo-service-checks

Runs codegen, verifies that no files are changed (git status is clean)

Runs apollo with the specified input parameters to either check or publish a schema

Inputs:

```
action: 'service:push' or 'service:check'
localSchemaFile: path to .graphql file
key: apollo key
variant: current env
serviceName: name of service
serviceURL: url of deployed running service (only required for service:push action)

codegenScript: script used to run codegen. default: npm run codegen
codegenGitStatusPaths: this action will run 'git status ${codegenGitStatusPaths}' and fail on any file changes. default: ''
```

Usage:

```
steps:
  - name: Checkout source
    uses: actions/checkout@v2

  - name: Set env vars
    uses: aplaceformom/actions-set-env-vars
    id: set-env-vars

  - name: Check schema
    uses: aplaceformom/actions-apollo-service-checks@main
    with:
      action: service:check
      localSchemaFile: src/schema.graphql
      key: ${{ steps.set-env-vars.outputs.APOLLO_KEY }}
      variant: ${{ steps.set-env-vars.outputs.APP_ENV J}}
      serviceName: my-service

  - name: Push schema
    uses: aplaceformom/actions-apollo-service-checks@main
    with:
      action: service:push
      localSchemaFile: src/schema.graphql
      key: ${{ steps.set-env-vars.outputs.APOLLO_KEY }}
      variant: ${{ steps.set-env-vars.outputs.APP_ENV J}}
      serviceName: my-service
      serviceURL: https://my-service.internal.${{ steps.set-env-vars.outputs.APP_ENV }}.aplaceformom.com/graphql
```
