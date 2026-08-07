# Integrating LLMs from Different AI Platforms with LangChain

In this notebook, we learned how to access and integrate LLMs from different AI providers using LangChain. We explored both hosted APIs and local Hugging Face models and used them as chat models.

### Platforms Covered

- Google AI Studio (Gemini)
- OpenAI (GPT)
- Anthropic (Claude - integration concept)
- Hugging Face Hosted Models
- Hugging Face Local Models
- Google Vertex AI (concept)

### LangChain Chat Classes

| Provider | LangChain Class |
|----------|-----------------|
| Google AI Studio | `ChatGoogleGenerativeAI` |
| OpenAI | `ChatOpenAI` |
| Anthropic | `ChatAnthropic` |
| Vertex AI | `ChatVertexAI` |
| Hugging Face Hosted | `ChatHuggingFace` |
| Hugging Face Local | `HuggingFacePipeline` |

### Key Takeaway

Although each provider has its own authentication method and model names, LangChain provides a consistent interface (`invoke()`, `stream()`, etc.), making it easy to switch between different LLM providers.
