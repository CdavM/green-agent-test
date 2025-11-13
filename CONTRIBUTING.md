# Contributing to This Benchmark

Thank you for your interest in submitting your agent to this benchmark! This guide explains how to run evaluations and submit your results.

## Submission Overview

### What You'll Submit

Your submission consists of one file in a pull request:
- `scores.json`: Your agent's performance metric

Also include a link to the workflow run in your purple agent repo that produced this output file

### Where to Submit

All submissions go in the `submissions/` directory with this structure:
```
submissions/<your-github-username>/<run-id>/
├── scores.json
```

**Example:**
```
submissions/alice/20250110-abc123/
├── scores.json
```

## Step-by-Step Submission Process

### 1. Run the Evaluation

Run the evaluation in **your own repository's CI environment** using:
- Your own compute resources
- Your own LLM API keys
- This repository's green agent (pinned by Docker digest)

See your white agent repository's documentation for how to configure and run evaluations.

### 2. Download the Results

After your CI run completes:
1. Download the artifacts from your GitHub Actions run
2. Locate the `scores.json` file
3. Verify the file is valid (see validation section below)

### 3. Create a Pull Request

1. Fork this repository
2. Create a new directory: `submissions/<your-username>/<run-id>/`
   - Use your GitHub username
   - Use a unique run ID (format: `YYYYMMDD-<identifier>`, e.g., `20250110-abc123`)
3. Add your `scores.json` file to this directory
4. Create a pull request with the title: `Submission: <your-username>/<run-id>`

### 4. PR Description

Include in your PR description:
- **Agent name**: Name of your agent
- **Run URL**: Link to your GitHub Actions run
- **Notes**: Any relevant information about this submission (optional)

**Example PR description:**
```markdown
## Submission Details

- **Agent**: MyAwesomeAgent
- **Run URL**: https://github.com/alice/my-agent/actions/runs/123456789
- **Notes**: First submission using GPT-4 as the backend

## Checklist
- [x] Files are in correct directory structure
- [x] scores.json matches the schema
- [x] Run URL is accessible
```

## File Format Requirements

### scores.json

Key requirements:
- **Required field**: `ID` (UUID of the agent being evaluated)
- Additional fields can be customized based on the benchmark
- Must be valid JSON

## Code of Conduct

- Submit only legitimate evaluation results
- Do not submit results from modified green agents or bypassed evaluations
- One submission per evaluation run (no duplicates)
- Be respectful in all interactions

Thank you for contributing to this benchmark!
