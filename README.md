# 🚀 Day 14/90 — Docker CI/CD with GitHub Actions & Trivy

## 📌 Project Overview

This project demonstrates a Docker-based CI/CD pipeline using **GitHub Actions, Docker Hub, and Trivy**. The pipeline automatically validates the Dockerfile, builds a Docker image, tests the application's health endpoint, scans the image for security vulnerabilities, and publishes the image to Docker Hub when changes are pushed to the `main` branch.

This hands-on project is part of my **90-Day DevOps Winter Arc Challenge**.

## 🎯 Objectives

- Automate Docker image builds using GitHub Actions.
- Validate Dockerfile best practices using Hadolint.
- Test the application using a health-check endpoint.
- Scan container images for HIGH and CRITICAL vulnerabilities using Trivy.
- Publish versioned Docker images to Docker Hub.
- Use Git commit SHA tags for traceable image versions.

## 🛠️ Technologies Used

- Docker
- Docker Hub
- Git and GitHub
- GitHub Actions
- Flask and Python
- Hadolint
- Trivy

## 🏗️ CI/CD Pipeline

```text
Developer Push
      |
      v
GitHub Repository
      |
      v
GitHub Actions Workflow
      |
      v
Dockerfile Linting
      |
      v
Build Docker Image
      |
      v
Application Smoke Test
      |
      v
Trivy Security Scan
      |
      v
Docker Hub Login
      |
      v
Tag Image with Commit SHA
      |
      v
Push Image to Docker Hub
```

**Note:** Docker Hub publishing runs only for pushes to `main`. Pull requests run the validation, build, smoke-test, and security-scan stages.

## ⚙️ Pipeline Stages

### 1. Checkout Source Code

Downloads the repository code into the GitHub Actions runner.

### 2. Dockerfile Linting

Uses Hadolint to detect Dockerfile issues and recommend best practices.

### 3. Docker Image Build

Builds the application image using Docker Buildx.

### 4. Application Smoke Test

Starts the container and checks the `/health` endpoint to verify that the application responds correctly.

### 5. Trivy Security Scan

Scans the built image for HIGH and CRITICAL vulnerabilities. The workflow fails if matching vulnerabilities are found under the configured scan policy.

### 6. Docker Hub Authentication

Uses GitHub Actions secrets to authenticate securely with Docker Hub.

### 7. Image Tagging and Publishing

Tags the image using the Git commit SHA and pushes it to Docker Hub.

## 🔐 GitHub Actions Secrets

Configure these secrets under **GitHub Repository → Settings → Secrets and variables → Actions**.

| Secret               | Purpose                 |
| -------------------- | ----------------------- |
| `DOCKERHUB_USERNAME` | Docker Hub username     |
| `DOCKERHUB_TOKEN`    | Docker Hub access token |

Never commit access tokens or passwords to the repository.

## 🐳 Docker Hub Image

**Docker Hub repository:**
[https://hub.docker.com/r/poojathakre08/winter-arc-app](https://hub.docker.com/r/poojathakre08/winter-arc-app)

Images are tagged with the commit SHA to help identify and reproduce a specific build.

Example:

```bash
docker pull poojathakre08/winter-arc-app:<COMMIT_SHA>
docker run -d --name day14-test -p 5001:5000 poojathakre08/winter-arc-app:<COMMIT_SHA>
curl -i http://localhost:5001/health
```

Replace `<COMMIT_SHA>` with the actual tag published by your workflow.

## 📸 Screenshots

Add your screenshots to the `screenshots/` folder.

|                                 | Screenshot                         | Description |
| ------------------------------- | ---------------------------------- | ----------- |
| `01-github-actions-success.png` | Successful GitHub Actions workflow |             |
| `02-dockerhub-image.png`        | Published Docker Hub image and tag |             |
| `03-trivy-scan.png`             | Trivy security scan logs           |             |
| `04-health-check.png`           | Application health-check response  |             |

> Add screenshots after capturing the actual results from your environment.

## 🔄 Rollback Strategy

If a newly published image causes an issue:

1. Identify the previous known-good commit SHA tag in Docker Hub.
2. Pull that specific image tag.
3. Stop the affected container.
4. Deploy the previous image tag.
5. Verify the `/health` endpoint and application behavior.

Example:

```bash
docker pull poojathakre08/winter-arc-app:<PREVIOUS_COMMIT_SHA>
docker run -d --name winter-arc-rollback -p 5001:5000 poojathakre08/winter-arc-app:<PREVIOUS_COMMIT_SHA>
curl -i http://localhost:5001/health
```

Replace `<PREVIOUS_COMMIT_SHA>` with a real previously published tag. This example demonstrates a manual rollback; production deployments should also account for traffic switching, container cleanup, and data compatibility.

## 📚 Key Learnings

- Creating automated CI/CD workflows with GitHub Actions.
- Building and testing Docker images.
- Validating Dockerfiles with Hadolint.
- Scanning container images with Trivy.
- Managing Docker Hub credentials using GitHub secrets.
- Versioning images using Git commit SHA tags.
- Understanding image publishing and rollback strategies.

## 🔮 Future Improvements

- Add automated unit tests.
- Generate and publish machine-readable security reports.
- Add image signing and software bill of materials (SBOM).
- Deploy validated images to an AWS environment.
- Add deployment approvals and automated rollback.

## 👩‍💻 Author

**Pooja Thakre**

GitHub: [https://github.com/poojathakre08](https://github.com/poojathakre08)

Project Repository: [https://github.com/poojathakre08/day14-docker-cicd](https://github.com/poojathakre08/day14-docker-cicd)

---

⭐ If you find this project useful, feel free to explore the repository and share feedback.

              