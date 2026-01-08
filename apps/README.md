# Application Deployment Guide

## How to Deploy Your Service

### 1. Create Your App Directory

```bash
mkdir apps/your-app-name
cd apps/your-app-name
```

### 2. Add Your Kubernetes Manifests

Create these files:

- `deployment.yaml` - Your application deployment
- `service.yaml` - Service to expose your app
- `kustomization.yaml` - Kustomize configuration

### 3. Follow the Pattern

See `hello-app/` for a complete example.

**Required:**

- All resources must target `namespace: apps`
- Include resource limits and requests
- Use meaningful labels

### 4. Deploy via Git

```bash
git add apps/your-app-name/
git commit -m "Add new service: your-app-name"
git push origin main
```

### 5. Monitor Deployment

- ArgoCD will automatically detect and deploy your app
- Check ArgoCD UI for sync status
- Verify pods are running: `kubectl get pods -n apps`

## What You Own vs Platform Team

### You Own (Application Team)

- Application code and images
- Kubernetes manifests in your app folder
- Application configuration and secrets
- Service dependencies

### Platform Owns

- Namespace creation and policies
- ArgoCD configuration
- Cluster infrastructure
- Deployment standards and guardrails

## Rollback Process

1. Revert your Git commit
2. ArgoCD will automatically sync the previous state
3. Or use ArgoCD UI to rollback to previous revision

## Need Help?

- Check existing apps for patterns
- Review platform documentation
- Contact platform team for infrastructure issues
