# Green Agent Dockerfile Template
#
# This Dockerfile defines the container for your green agent evaluation orchestrator.
# Customize this file based on your implementation language and dependencies.

# TODO: Choose an appropriate base image for your implementation
# Examples:
# - Python: FROM python:3.11-slim
# - Node.js: FROM node:20-slim
# - Go: FROM golang:1.21-alpine AS builder
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# TODO: Copy and install your dependencies
# For Python example:
# COPY requirements.txt .
# RUN pip install --no-cache-dir -r requirements.txt

# TODO: Copy your evaluation code
# COPY . .

# Environment variables for agent discovery
# The green agent will receive agent URLs via environment variables:
# - WHITE_AGENT_URL: URL of the white agent being evaluated (required)
# - BASELINE_AGENT_URLS: JSON array of baseline agent URLs (optional)
#   Example: BASELINE_AGENT_URLS='["http://baseline1:8080", "http://baseline2:8080"]'
#
# Alternative: Mount a configuration file at /config/agents.json:
# {
#   "white_agent": {"url": "http://white-agent:8080"},
#   "baselines": [
#     {"name": "baseline_v1", "url": "http://baseline1:8080"}
#   ]
# }

# Output directory for results
ENV OUTPUT_DIR=/output

# Create output directory
RUN mkdir -p /output

# TODO: Add health check endpoint (optional but recommended)
# This allows orchestration systems to verify the container is ready
# HEALTHCHECK --interval=10s --timeout=3s --start-period=5s --retries=3 \
#   CMD curl -f http://localhost:8080/health || exit 1

# TODO: Set the entrypoint to your evaluation command
# This should:
# 1. Read agent URLs from environment variables or /config/agents.json
# 2. Run the evaluation
# 3. Output scores.json (with required ID field) to $OUTPUT_DIR
#
# Examples:
# - Python: CMD ["python", "green.py"]
# - Node.js: CMD ["node", "green.js"]
# - Go: CMD ["./green"]
# - Shell script: CMD ["./run_evaluation.sh"]

CMD ["echo", "TODO: Replace this with your evaluation command"]
