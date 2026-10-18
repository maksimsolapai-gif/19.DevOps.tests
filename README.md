# 19.DevOps.tests

## Repository Links
* **Project and Docker Tests Repository:** https://github.com/maksimsolapai-gif/09.Docker.Basic/tree/master/homework3/
* **Kubernetes Manifests Repository:** https://github.com/maksimsolapai-gif/argo-flux-36-26/tree/master/monitoring/
---

## Homework Assignment 1: Automate Docker images test by github action

### Implemented Features (Workflow: `.github/workflows/docker-tests.yml`):
* **Build and Deploy:** Automated construction of the Docker image based on the Flask application located in the `homework3/` directory, followed by container execution with port forwarding (`5000:5000`).
* **HTTP Testing:** Validation of web application availability inside the container by performing a `curl` request. The test succeeds upon receiving an HTTP status code of `200 OK`.
* **Security (Non-root access):** Enforcement of process boundaries. The `Dockerfile` includes a `USER 10001` directive to guarantee that the application runs without root privileges.

<img width="975" height="602" alt="image" src="https://github.com/user-attachments/assets/ff256ae0-f07b-4789-8577-feccf1b72ba7" />

<img width="975" height="659" alt="image" src="https://github.com/user-attachments/assets/00ef5cf4-71d9-4a1d-9ace-454cfbe3e279" />

---

## Homework Assignment 2: Create CI for testing Kubernetes manifests using kubeconform by github action

### Implemented Features (Workflow: `.github/workflows/kubeconform-tests.yml`):
* **Pipeline Triggers:** Automated execution configured to trigger on any `push` event or `Pull Request` target branch `master`.
* **Tooling Isolation:** Validation steps run inside the official `kubeconform` Docker container context on the GitHub Runner, eliminating any third-party dependencies on the host environment.
* **Parallel Validation:** Multi-threaded syntax and schema verification implemented across all Kubernetes YAML resources within the target `monitoring/` directory using the `-workers 4` argument.

```
      - name: Run Kubeconform validation
        run: |
          docker run --rm -v ${{ github.workspace }}:/apps -w /apps ghcr.io/yannh/kubeconform:latest-alpine \
            -summary \
            -verbose \
            -workers 4 \
            -strict \
            monitoring/
```

* **Notification Integration:** Automated transmission of deployment and validation metrics to external communication channels (Telegram/Slack) utilizing GitHub Secrets for credential isolation.

<img width="1511" height="880" alt="image" src="https://github.com/user-attachments/assets/961098a7-7fb2-4186-9996-2743323566c1" />

<img width="975" height="358" alt="image" src="https://github.com/user-attachments/assets/6ffae9b8-a9a8-4859-b918-c24d36cb58f5" />

<img width="975" height="358" alt="image" src="https://github.com/user-attachments/assets/1bf68869-6cf7-4e2a-813b-58b526671ea9" />

---
