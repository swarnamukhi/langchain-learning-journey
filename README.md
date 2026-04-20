# LangChain Learning Journey

This repository documents my learning journey in Generative AI using Google AI Studio and LangChain.

## 📌 Topics Covered

- Google Generative AI SDK (Direct usage)
- LangChain integration with Gemini models
- Authentication methods (API key vs environment variables)
- Basic prompt interaction with LLMs

---

## 🔹 Approach 1: Without LangChain (SDK)

In this approach, I used the `google-generativeai` SDK to directly interact with Gemini/Gemma models.

- Configured API using `genai.configure()`
- Used `GenerativeModel` to send prompts
- Direct control over model interaction

---

## 🔹 Approach 2: With LangChain

In this approach, I used LangChain as a wrapper over Google models.

- Used `ChatGoogleGenerativeAI`
- Authentication via environment variable
- Standardized interface for LLM interaction

---

## ⚖️ SDK vs LangChain

| Feature | SDK | LangChain |
|--------|-----|----------|
| Control | High | Moderate |
| Memory | ❌ | ✔ |
| Agents | ❌ | ✔ |
| RAG | Manual | Supported |

---

## 🎯 Goal

To build a strong foundation in:
- Generative AI
- LangChain
- Real-world AI applications

---

## 🚀 Next Steps

- Add memory to chatbot
- Build RAG pipeline
- Integrate with real datasets