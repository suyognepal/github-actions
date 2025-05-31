# GitHub Actions Automation Suite

## Overview

This repository provides a modular, secure, and scalable set of GitHub Actions for building, validating, deploying, and notifying across multiple environments and technology stacks. It is designed for modern DevOps workflows, supporting containerized, VM-based, and Kubernetes-based deployments, with integrated secret management and notification capabilities.

## Directory Structure

```
github-actions/
  actions/
    build/      # Build automation for Docker, Next.js, Go, .NET, etc.
    deploy/     # Deployment automation for Docker, Kubernetes, and VMs
    notify/     # Notification integrations (e.g., Slack)
    validate/   # Validation and pre-deployment checks
  workflow/
    deployment.yaml  # Example workflow using the provided actions
  Readme.md
```

## Features

- **Modular Build Actions**: Build and push Docker images for various stacks (Go, .NET, Next.js) with Vault-based secret management.
- **Flexible Deployment**:
  - **Kubernetes**: Helm-based deployments for Go, .NET, and Next.js, with support for Istio, secret injection, and robust validation.
  - **Docker**: Remote Docker image deployment with secure SSH and Vault integration.
  - **VM**: Rsync and Docker Compose-based deployments to VMs, with optional encryption and secret management.
- **Validation**: Pre-deployment validation steps to ensure build integrity.
- **Notifications**: Customizable Slack notifications for build and deployment results.
- **Security**: All secrets are managed via HashiCorp Vault, following the principle of least privilege.
- **Extensibility**: Easily add new stacks or notification channels by following the existing modular structure.

## Usage

### Example Workflow

See [`workflow/deployment.yaml`](workflow/deployment.yaml) for a complete example. The workflow includes:

- **Build**: Uses `actions/build/docker/dotnet` to build and push a Docker image.
- **Deploy**: Uses `actions/deploy/k8s/dotnet` to deploy to Kubernetes.
- **Notify**: Uses `actions/notify/slack` to send deployment notifications to Slack.

### Action Inputs

All actions are parameterized and require environment variables or secrets, typically injected from Vault. Common inputs include:

- `VAULT_SERVER`, `VAULT_TOKEN`: Vault connection details.
- `VAULT_SECRETS_CICD_PATH`, `VAULT_SECRETS_PATH`: Paths to CI/CD and application secrets.
- `ENVIRONMENT`, `IMAGE_TAG`, `PROJECT_NAME`: Build/deployment context.
- Stack-specific options (e.g., `ISTIO`, `REPLICAS`, `ENCRYPTION`).

See each action's `action.yml` or `action.yaml` for full input documentation.

### Security

- **Secrets**: All sensitive data is pulled from Vault at runtime.
- **SSH**: Key-based authentication is used for all remote operations.
- **Cleanup**: Actions clean up sensitive files and environment variables after use.

### Extending

To add a new stack or deployment target:

1. Copy an existing action as a template.
2. Update the logic and inputs as needed.
3. Document the new action in its own `action.yml`/`action.yaml`.
4. Reference it in your workflow.

## Best Practices

- Use environment variables and Vault for all secrets.
- Parameterize all actions for reusability.
- Validate all inputs and handle errors gracefully.
- Use tags and modular roles for flexible execution.
- Follow GitOps and Infrastructure-as-Code principles.

## Contributing

1. Fork the repository and create a feature branch.
2. Add or update actions, ensuring modularity and security.
3. Test your changes using sandbox environments.
4. Submit a pull request with clear documentation.

---

**Note:** For detailed usage of each action, refer to the respective `action.yml`/`action.yaml` files under the `actions/` directory.
