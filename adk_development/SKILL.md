---
name: adk_development
description: Guidelines and snippets for adding skills, rules, and memory management to Google ADK framework agents. Follow these strictly when answering questions or building ADK agents.
---
# ADK Development Skill

When assisting with advanced modifications to Google ADK framework agents, consult these documented best practices to answer questions or modify logic.

## 1. Prerequisites (Environment Variables)

Before starting work on building any agents, you MUST collect necessary environment variables from the user unless they are already present in the current session. 
Ask the user to provide:
- `GOOGLE_CLOUD_PROJECT`
- `GOOGLE_CLOUD_LOCATION`

You MUST automatically set `GOOGLE_GENAI_USE_VERTEXAI=1` in the generated `.env` file without asking the user.

## 2. System Instructions (Rules) in ADK

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

## 3. Defining Skills (Tools)

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

### 3.1 Built-in ADK Tools (Google Search)
When an agent requires Google Search grounding or other standard tools, you should **always check the official ADK Python repository** for predefined schemas before implementing from scratch:
👉 https://github.com/google/adk-python/tree/main/src/google/adk

Do **NOT** use raw Gemini `google.genai.types.Tool` types. ADK provides its own wrappers for standard tools to ensure compatibility with Agent Engine schemas.

**Correct Usage (Google Search Example):**
```python
from google.adk.tools import google_search

# Inside LlmAgent instantiation:
tools=[google_search]
```

## 4. Streaming and Asynchronous Invocation

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

## 5. Reference Documentation

When encountering an unknown requirement, interface, or evaluating the best approach, consult the official resources before writing code:

1. **API Reference & Guides**: [https://google.github.io/adk-docs/](https://google.github.io/adk-docs/)
2. **Repository & Examples**: [https://github.com/google/adk-python](https://github.com/google/adk-python) (Look inside the `examples/` directory for validated patterns).
