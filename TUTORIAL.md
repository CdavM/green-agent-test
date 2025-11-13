# Green Agent Maintainer Tutorial

This tutorial guides you through setting up your own AI agent benchmark using this green agent starter template.

## Table of Contents

1. [Introduction](#1-introduction)
2. [Prerequisites](#2-prerequisites)
3. [Repository Setup](#3-repository-setup)
4. [Define Your Schema](#4-define-your-schema)
5. [Testing Your Benchmark](#5-testing-your-benchmark)

---

## 1. Introduction

### What You'll Build

By the end of this tutorial, you'll have:
- A working green agent that evaluates AI agents on your custom benchmark
- A documented JSON schema for evaluation results
- Published Docker images with pinned digests for reproducibility
- Complete documentation for white agent developers to submit to your benchmark

### What is a Green Agent?

A green agent is the **evaluation orchestrator** for your benchmark. It:
- Defines what tasks agents should perform
- Communicates with white agents (the agents being evaluated) via the A2A protocol
- Computes performance metrics
- Outputs standardized results (JSON files)

### What is a White Agent?

White agents are **the AI agents being evaluated**. They:
- Implement the A2A protocol to receive tasks from your green agent
- Run in their own CI environments (their compute, their API keys)
- Submit results to your repository via pull request

---

## 2. Prerequisites

Before starting, ensure you have:

### Required Knowledge
- Basic programming experience
- Docker fundamentals
- Git and GitHub
- Understanding of your evaluation domain

### Required Tools
- Docker installed locally
- GitHub account with access to GitHub Container Registry (ghcr.io)
- Text editor or IDE

### Optional Tools
- Python 3.9+ (for working with JSON files)
- `jq` for inspecting and querying JSON files

---

## 3. Repository Setup

### 3.1 Clone or Fork This Template

```bash
# Option 1: Clone this template
git clone https://github.com/your-org/green-agent-starter-repo.git my-benchmark
cd my-benchmark

# Option 2: Use GitHub's "Use this template" button
# Then clone your new repository
```

### 3.2 Understand the Directory Structure

```
my-benchmark/
├── README.md              # Update with your benchmark description
├── CONTRIBUTING.md        # Submission guidelines (update with specifics)
├── TUTORIAL.md           # This file
├── eval/                 # Your evaluation code goes here
│   ├── green.py         # Main orchestrator - IMPLEMENT THIS
├── schema.json          # JSON schema definition - DEFINE THIS
├── submissions/         # PRs will add results here
└── eval-results/        # Aggregated results (future work)
```

### 3.3 Implement your Green Agent

Implement your green agent to be able to evaluate various purple agents! This code should live in the `eval` directory, especially if you want Github to automatically build and push your green agent Docker image so that it's accessible to white agent developers easily.

### 3.4 Build and Publish Your Green Agent Docker Image

White agents need to pull your green agent image to run evaluations. You'll publish it to [GitHub Container Registry (GHCR)](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry).

**⚠️ Important**: Always build for `linux/amd64` architecture (used by GitHub Actions), even if you're on an M1/M2 Mac or ARM machine.

#### Option A: GitHub Actions (Recommended)

This repository includes a GitHub Actions workflow at `.github/workflows/publish-green-agent.yml` that automatically builds and publishes your Docker image.

**How it works:**
- **On PRs**: Builds the image when you modify files in `eval/` (validates the build works)
- **On push to main**: Builds and pushes with tag `latest`
- **On version tags**: Builds and pushes with semantic version tags (e.g., `v1.0` → tags `1.0`, `1`)

**GitHub Token Setup:**
The `GITHUB_TOKEN` secret is automatically provided by GitHub Actions - no manual configuration needed! The workflow has `packages: write` permission to push to GitHub Container Registry.

**To publish a new version:**

```bash
# Commit your changes to eval/
git add eval/
git commit -m "Update green agent"

# Create and push a version tag
git tag v1.0
git push origin v1.0
```

The workflow will automatically build for `linux/amd64` and push to `ghcr.io/YOUR_ORG/YOUR_REPO:1.0`. Check the Actions tab for the build log and image digest.

#### Option B: Build and Push Locally

```bash
# Authenticate to GHCR
echo $GITHUB_TOKEN | docker login ghcr.io -u YOUR_USERNAME --password-stdin

# Build for linux/amd64 (important!)
docker buildx build \
  --platform linux/amd64 \
  -t ghcr.io/your-org/my-benchmark:v1.0 \
  --push \
  eval/
```

**Note**: You need a [GitHub Personal Access Token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) with `write:packages` scope for `GITHUB_TOKEN`.

#### Get the Image Digest

The digest ensures reproducibility. After pushing:

```bash
docker buildx imagetools inspect ghcr.io/your-org/my-benchmark:v1.0 --format '{{json .Manifest.Digest}}'
```

Save this digest (e.g., `sha256:abc123...`) - white agents will reference it.

#### Make Your Image Public

By default, GHCR packages are private. To make it public:

1. Go to `https://github.com/orgs/YOUR_ORG/packages`
2. Click your package → Package settings
3. Scroll to "Danger Zone" → Change visibility → Public

See [GitHub's package visibility docs](https://docs.github.com/en/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility).

#### Alternative Registries

You can also use:
- [Docker Hub](https://hub.docker.com/)
- [AWS ECR Public](https://docs.aws.amazon.com/AmazonECR/latest/public/what-is-ecr.html)
- Any OCI-compatible registry

The key is publishing with a digest that white agents can pin.

---

## 4. Testing Your Benchmark

### 4.1 Test Locally

```bash
# Build green agent
docker build -t green-agent:test eval/

# Run a mock white agent (for testing)
# (Create a simple test agent or use a baseline)
docker run -d --name test-white-agent -p 8080:8080 your-test-agent

# Run green agent evaluation
docker run --rm \
  -e WHITE_AGENT_URL=http://host.docker.internal:8080 \
  -e SCENARIO=default \
  -v $(pwd)/out:/output \
  green-agent:test

# Check output
ls out/
# Should see: scores.json, MANIFEST.json
```

### 4.2 Create Test Submission

Test the full submission workflow:

1. Run evaluation with a test agent
2. Create a test submission PR to your own repo
3. Verify directory structure is correct
4. Test that files are readable and valid

---

## Next Steps

You now have the foundation for a green agent benchmark! Next:

1. **Implement your evaluation logic** in `eval/green.py`
2. **Create baseline agents** for reference performance
3. **Announce your benchmark**: Share with the community
4. **Accept submissions**: Review and merge PRs from white agent developers
5. **Maintain**: Update baselines, fix issues, improve documentation

## Getting Help

- Review the example green agent code in the `sample-debate-green-agent` repo
- Check `CONTRIBUTING.md` for submission guidelines
- Open an issue if you encounter problems

Good luck with your benchmark!
