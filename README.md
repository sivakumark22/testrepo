# testrepo

Minimal Spring Boot service and GitHub Actions CI/CD pipeline to:

1. Build and test with Maven
2. Build and push Docker image to Azure Container Registry (ACR)
3. Deploy to AKS `dev`, `test`, `stage`, and `prod` environments
4. Enforce approval gates via GitHub Environments (`stage`, `prod`)

## Spring Boot app

- Java 17
- Spring Boot 3
- `/health` endpoint for Kubernetes probes

Run locally:

```bash
mvn clean test package
java -jar target/testrepo-0.0.1-SNAPSHOT.jar
```

## CI/CD workflow

Workflow file: `.github/workflows/cicd-aks.yml`
Code scanning workflow: `.github/workflows/codeql.yml`

Pipeline order:

- `build-and-push`
- `deploy-dev`
- `deploy-test`
- `deploy-stage`
- `deploy-prod`

`deploy-stage` and `deploy-prod` use GitHub `environment` blocks (`stage`, `prod`).
Set required reviewers in repository **Settings → Environments** to enable approval gates.

## Required GitHub Secrets

Repository secrets:

- `AZURE_CLIENT_ID`
- `AZURE_TENANT_ID`
- `AZURE_SUBSCRIPTION_ID`
- `ACR_NAME`
- `ACR_LOGIN_SERVER`
- `AKS_RESOURCE_GROUP_DEV`
- `AKS_CLUSTER_NAME_DEV`
- `AKS_RESOURCE_GROUP_TEST`
- `AKS_CLUSTER_NAME_TEST`
- `AKS_RESOURCE_GROUP_STAGE`
- `AKS_CLUSTER_NAME_STAGE`
- `AKS_RESOURCE_GROUP_PROD`
- `AKS_CLUSTER_NAME_PROD`

## Kubernetes manifests

Base manifests are in `k8s/` and are applied to namespace:

- `dev`
- `test`
- `stage`
- `prod`
