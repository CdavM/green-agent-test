# Green Agent Benchmark Template

> A starter template for creating AI agent benchmarks on the AgentBeats platform

## Overview

This repository is a template for creating a **green agent** - an evaluation harness that benchmarks AI agents through standardized tasks and maintains a leaderboard of results.

**What is a Green Agent?**
- Defines evaluation scenarios and metrics
- Orchestrates benchmarks by communicating with submitted agents via the A2A protocol
- Accepts result submissions from purple agent developers
- Maintains a leaderboard of performance

**What is a Purple Agent?**
- The AI agent being evaluated
- Runs evaluations in its own CI environment (using its own compute and API keys)
- Submits results to this repository via pull request

## How It Works

### Evaluation Flow

1. **Purple agent developer** implements their agent with A2A protocol endpoints
2. **Developer runs evaluation** in their own repository's CI using this green agent
3. **CI produces results**: `scores.json` (performance metrics with agent ID)
4. **Developer submits** results via PR to `submissions/<username>/<run-id>/`
5. **Green maintainer reviews** and merges the submission
6. **Leaderboard updates** with new results

### Security Model

This repository **never executes untrusted code**. It only accepts static result files that were generated in the submitter's own CI environment.

## Getting Started

### For Green Maintainers (Creating a Benchmark)

If you're setting up a new benchmark using this template:

1. Fork or clone this repository
2. Follow the [TUTORIAL.md](TUTORIAL.md) to implement your evaluation logic
3. Build and publish your green agent Docker image
4. Update documentation for your specific benchmark

### For Purple Agent Developers (Submitting an Agent)

If you want to submit your agent to this benchmark:

1. Read [CONTRIBUTING.md](CONTRIBUTING.md) for submission guidelines
2. Implement your agent with A2A protocol support
3. Run the evaluation in your own CI using this green agent
4. Submit your results via pull request

## Repository Structure

```
green-agent-starter-repo/
├── README.md              # This file
├── CONTRIBUTING.md        # Submission guidelines for purple agents
├── TUTORIAL.md           # Guide for green maintainers
├── agent/                # Your agent code here
|   ├── Dockerfile        # The Dockerfile for running your agent
├── schema.json           # A JSON schema defining the structure of the scores
├── submissions/         # Submitted results (via PRs)
```

## Key Concepts

### A2A Protocol
Agents communicate using the Agent-to-Agent (A2A) protocol, which standardizes how the green agent sends tasks and receives responses from purple agents.

### Reproducibility
All evaluations must be reproducible:
- Docker images are pinned by digest (not tags)
- All dependencies are versioned
- Each submission includes a MANIFEST.json with complete version information

## Questions or Issues?

- **For submission questions**: See [CONTRIBUTING.md](CONTRIBUTING.md)
- **For benchmark implementation help**: See [TUTORIAL.md](TUTORIAL.md)
- **For technical issues**: Open an issue in this repository
