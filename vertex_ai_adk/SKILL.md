---
name: Vertex AI ADK Setup
description: Skill for setting up an ADK agent to always use Vertex AI.
---

# Vertex AI ADK Setup

This skill describes how to correctly set up and configure an ADK Agent project for a user who strictly uses Vertex AI.

## Instructions

Whenever you work on an ADK project (or similar GenAI projects) in this environment:

0. **Check Prerequisites**: Explicitly ask the user for `GOOGLE_CLOUD_PROJECT` and `GOOGLE_CLOUD_LOCATION` if they are not already in the session. Do not proceed with code generation until they are provided.

1. **Verify `.env` Existence**: Ensure there is a `.env` and `.env.example` file.
2. **Force Vertex AI**: The environment variable `GOOGLE_GENAI_USE_VERTEXAI=1` MUST be automatically included in the `.env` file and any deployment config maps. Do not ask the user for it.
3. **Include Project Identity**: Ensure `GOOGLE_CLOUD_PROJECT` and `GOOGLE_CLOUD_LOCATION` are configured.
4. **Environment Loader**: Make sure the main `agent.py` or `app.py` script loads `.env` variables via `python-dotenv` at the very beginning of the file before `google.adk` imports.
5. **No API Keys**: Do not use `api_key` arguments for Vertex AI setups.

Follow these steps faithfully to avoid Google API authentication errors.
