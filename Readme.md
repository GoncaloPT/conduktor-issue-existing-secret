# Conduktor issue existing secret

## Pre-requisites
Have kind or any other k8s cluster running.

## Steps
create the secret using:

```shell
kubectl create secret generic kafka-ui-secret \
    --from-literal=CDK_ADMIN_EMAIL=admin \
    --from-literal=CDK_ADMIN_PASSWORD='benfica' \
    --from-literal=CDK_DATABASE_PASSWORD='benfica' \
    --from-literal=CDK_DATABASE_USERNAME='another' \
    --from-literal=CDK_ORGANIZATION_NAME='test-one' 
```

(optional - if not done already) download dependencies
```shell
cd tools-kafka-ui
helm dependency build ./
```

render the chart ( inside of tools-kafka-ui) folder, run:
```shell
helm template . --name-template tools-kafka-ui --values ./values.yaml --debug
```

## Error
After executing the previous step, The error  appears:

```text
Error: execution error at (tools-kafka-ui/charts/console/templates/platform/secret-credentials.yaml:18:19): config.organization.name MUST be set in values
helm.go:84: [debug] execution error at (tools-kafka-ui/charts/console/templates/platform/secret-credentials.yaml:18:19): config.organization.name MUST be set in values
```
This shouldn't happen because the secret exists, and have the expected keys/values:
The command:
```shell
kubectl get secret kafka-ui-secret -o jsonpath='{.data}'
```

produces the output:
```json
{
  "CDK_ADMIN_EMAIL":"YWRtaW4=",
  "CDK_ADMIN_PASSWORD":"YmVuZmljYQ==",
  "CDK_DATABASE_PASSWORD":"YmVuZmljYQ==",
  "CDK_DATABASE_USERNAME":"YW5vdGhlcg==",
  "CDK_ORGANIZATION_NAME":"dGVzdC1vbmU="
}
```
