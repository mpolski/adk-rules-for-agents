# ADK Rules for Agents

This repository contains a set of AI Agent Skills and Rules designed specifically to enforce best practices and standardize the development of Google Agent Development Kit (ADK) applications.

## What it is for
When you are building a new AI agent or application using the ADK framework, your personal AI coding assistant needs context on how to structure the code, what frameworks to use, and what environment parameters are required. This repository serves as the "brain" or instruction manual for your AI assistant, guaranteeing that it writes code aligning perfectly with your requirements.

## How to use it
To use these rules in a new project, simply copy or clone the contents of this repository into the `.agent/skills` directory of your new ADK project's root folder.

```bash
# Example setup for a new project
mkdir -p .agent/skills
git clone https://github.com/mpolski/adk-rules-for-agents.git .agent/skills/
```

When you open the new project workspace in your IDE and prompt your AI assistant, it will automatically detect these `.md` files and abide by their defined behavioral logic.

## Supported Frameworks & Dependencies
These rules are tailored for:
- **Framework**: Google Agent Development Kit (ADK)
- **Primary AI Provider**: Google Vertex AI
- **Key Dependencies**:
  - `google-cloud-aiplatform` (For Vertex AI interactions)
  - `python-dotenv` (For local environment configuration loading)

## What these rules do
Once the AI assistant reads these rules, it will automatically enforce the following behaviors during code generation and project setup:

1. **Vertex AI Enforcement**: It will default to using Vertex AI endpoints instead of public AI Studio endpoints by automatically generating `.env` files and deployment scripts containing `GOOGLE_GENAI_USE_VERTEXAI=1`.
2. **Environment Pre-requisites**: It will proactively pause and ask you to provide your `GOOGLE_CLOUD_PROJECT` and `GOOGLE_CLOUD_LOCATION` before it begins scaffolding the agent infrastructure.
3. **No API Keys**: It will strictly avoid using raw API key parameters, relying fully on Google Cloud Application Default Credentials (ADC) and Vertex AI configuration.
4. **Agent Best Practices**: It provides the AI with the optimal design patterns for the ADK framework, including how to structure `system_instructions` and how to properly document and type custom Tools/Skills.
5. **Streaming Configurations**: It instructs the AI on how to correctly configure the `deploy.py` manifest to support streaming capabilities correctly using `api_mode: "stream"`.
