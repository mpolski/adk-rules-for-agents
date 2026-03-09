---
name: Vertex AI ADK Setup
description: Skill for setting up an ADK agent to always use Vertex AI.
---

# Vertex AI ADK Setup

This skill describes how to correctly set up and configure an ADK Agent project for a user who strictly uses Vertex AI.

## Instructions

Whenever you work on an ADK project (or similar GenAI projects) in this environment:

1. **Collect Prerequisites**: Before starting work on building any agents, ask the user to provide necessary environment variables (`GOOGLE_CLOUD_PROJECT`, `GOOGLE_CLOUD_LOCATION`, etc.) unless they are already in the session.
2. **Verify `.env` Existence**: Ensure there is a `.env` and `.env.example` file.
3. **Force Vertex AI**: Automatically set the environment variable `GOOGLE_GENAI_USE_VERTEXAI=1` in the `.env` file and any deployment config maps without prompting the user.
4. **Include Project Identity**: Ensure `GOOGLE_CLOUD_PROJECT` and `GOOGLE_CLOUD_LOCATION` are configured.
5. **Environment Loader**: Make sure the main `agent.py` or `app.py` script loads `.env` variables via `python-dotenv` at the very beginning of the file before `google.adk` imports.
6. **No API Keys**: Do not use `api_key` arguments for Vertex AI setups.

Follow these steps faithfully to avoid Google API authentication errors.
