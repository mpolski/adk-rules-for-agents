---
name: adk_development
description: Guidelines and snippets for adding skills, rules, and memory management to Google ADK framework agents. Follow these strictly when answering questions or building ADK agents.
---
# ADK Development Skill

When assisting with advanced modifications to Google ADK framework agents, consult these documented best practices to answer questions or modify logic.

## 1. System Instructions (Rules) in ADK

To add personality, context, or rules to an ADK Agent, declare instructions when instantiating the LLM client or root agent, or design a custom Prompt class.

**Example Pattern:**
```python
class MyAgent:
    def __init__(self):
        self.system_instruction = "You are a helpful coding assistant. Always answer in Markdown."
    
    def ask(self, question: str) -> str:
        # Example pseudo-code; implementation will vary based on user's Vertex AI client choice within ADK.
        return f"{self.system_instruction}\nUser asked: {question}"
```

## 2. Defining Skills (Tools)

When attaching external functionalities to the LLM within an ADK project (like database lookups or external APIs), you create Python functions and expose them as tools. 
> Ensure typing and docstrings are immaculate, as the underlying platform uses them to generate schemas.

**Example Pattern:**
```python
def lookup_database(user_id: str) -> dict:
    \"\"\"Fetches user records from the main database.
    
    Args:
        user_id: The unique identifier for the user.
    \"\"\"
    return {"user_id": user_id, "status": "active"}

# Then, attach or pass this tool to your agent's underlying generative model.
```

## 3. Streaming and Asynchronous Invocation

Agent Engine supports streaming tokens using `async_stream` or `stream` mode.
When a user asks for streaming, ensure your `class_methods` inside the `deploy.py` exposes the generator function properly:
```python
"class_methods": [
    {
        "name": "ask_stream",
        "api_mode": "stream", # REQUIRED FOR STREAMING
        "parameters": { ... }
    }
]
```

## 3. Pre-requisites & Environment Variables
Before starting work to build new ADK agents, always check the current session context to see if the required environment variables are present. If `GOOGLE_CLOUD_PROJECT` and `GOOGLE_CLOUD_LOCATION` have not been provided, you MUST explicitly ask the user for them before writing any configuration or code.\n\nAdditionally, you must automatically set `GOOGLE_GENAI_USE_VERTEXAI=1` in the `.env` file and any deployment configurations.
