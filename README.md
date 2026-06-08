# ArgoCD-test

# Deploying a Sample App with Argo CD on Kind

## Introduction
In this guide, we walk through deploying a sample Nginx application using Argo CD on a local Kind cluster. By following a GitOps workflow, we ensure the application is managed declaratively via Git.

## Setting Up Kind Locally
1. Installed Docker Desktop and ensured it was running.
2. Verified Kind version with `kind version` to confirm installation.
3. Created a Kind cluster with:
   ```bash
   kind create cluster --name my-cluster --config kind-config.yaml
   ```

## Creating Kubernetes Manifests
1. Created a file named `nginx-deployment.yaml` to define an Nginx Deployment with 2 replicas, exposing port 80.
2. Created a file named `app.yaml` to define the Argo CD Application resource, linking it to the GitHub repository and the path of the manifests.

## Pushing to GitHub
1. Created a public GitHub repository named `ArgoCD-test`.
2. Added both `nginx-deployment.yaml` and `app.yaml` to the repository.
3. Committed and pushed all changes to the main branch.

## Installing Argo CD
1. Installed Argo CD by running:
   ```bash
   kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
   ```
2. Verified Argo CD by checking:
   ```bash
   kubectl get pods -n argocd
   ```

## Configuring Argo CD
1. Accessed the Argo CD UI by port-forwarding the Argo CD server:
   ```bash
   kubectl port-forward svc/argocd-server -n argocd 8080:443 
   ```
2. Logged into the Argo CD UI (using the default admin credentials).
3. Created a new application (e.g., `test-01`) in the Argo CD UI.
4. Set the GitHub repo URL, path to manifests, and destination namespace (e.g., default).
5. Enabled auto-sync, prune, and self-heal in the sync policy.

## Testing and Syncing
1. Made manual changes (e.g., scaled replicas down to 2 using `kubectl scale`).
2. Observed Argo CD auto-reconcile and scale back to 4.
3. Verified the application’s health and status in the Argo CD UI.

## Conclusion
We successfully deployed a sample application using Argo CD on a Kind cluster. By leveraging GitOps, we ensure that all changes made in Git are automatically reflected in the cluster, providing a stable and self-healing deployment process.
