# Use Python 3.11 slim image (pinned by digest for reproducibility)
FROM python@sha256:8e5a0e2b2b8b9c8f3e3b4a3e3b4a3e3b4a3e3b4a3e3b4a3e3b4a3e3b4a3e3b4a

WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy green agent files
COPY adk_debate_judge.py .
COPY debate_judge_common.py .

# Expose the agent port
EXPOSE 9009

# Set environment variables
ENV HOST=0.0.0.0
ENV PORT=9009

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:9009/')" || exit 1

# Run the green agent
ENTRYPOINT ["python", "adk_debate_judge.py"]
CMD ["--host", "0.0.0.0", "--port", "9009"]
