# Mini CRM — CI/CD Demo (Ubuntu)

Simple CRM demo app used to test the full CI/CD pipeline:

```
GitHub → Jenkins → Docker Build → Docker Hub → Kubernetes → Pod → Service (NodePort)
```

## What the app does
A tiny single-page CRM: add customers (name, email, phone), search them, delete them.
Data lives only in the browser session (no backend/database) — it's purely to test
the deployment pipeline, not meant for real data.

## Files
- `index.html` — the CRM page (served by nginx)
- `Dockerfile` — builds an nginx image serving index.html
- `k8s/deployment.yaml` — Kubernetes Deployment
- `k8s/service.yaml` — Kubernetes Service (NodePort 30080)
- `Jenkinsfile` — pipeline definition

## How it works
1. Push code to `main` branch on GitHub.
2. Jenkins picks up the change (via Poll SCM or manual "Build Now").
3. Jenkins builds a Docker image tagged with the build number.
4. Image is pushed to Docker Hub.
5. Jenkins applies the Kubernetes manifests, rolling out the new image.
6. App becomes available at `http://<server-ip>:30080`.

## Jenkins Requirements
- Credentials: `dockerhub-creds` (Docker Hub username/password)
- Jenkins container needs Docker socket access and a working kubeconfig + kubectl
- A Pipeline job pointing to this repo's `Jenkinsfile`
