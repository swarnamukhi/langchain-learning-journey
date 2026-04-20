# 📘 Lesson 0: Google AI Studio + Gemini SDK + LangChain

## 📌 Overview

This lesson documents my initial exploration of Generative AI using **Google AI Studio**, where I worked with Gemini models using both:

* Direct SDK approach
* LangChain integration

The goal was to understand how different approaches interact with LLMs before moving to production-level tools like Vertex AI.

---

## 🎯 Objective

* Understand how to interact with Gemini models
* Compare SDK vs LangChain approaches
* Learn authentication methods
* Build a foundation for advanced GenAI applications

---

## 🧠 Topics Covered

* Google Generative AI SDK (Direct usage)
* LangChain integration with Gemini models
* Authentication methods (API Key vs Environment Variables)
* Basic prompt interaction with LLMs

---

## 🔹 Approach 1: Without LangChain (SDK)

In this approach, I used the **google-generativeai SDK** to directly interact with Gemini models.

### Key Points:

* Configured API using `genai.configure()`
* Used `GenerativeModel` to send prompts
* Direct interaction with the model
* Full control over request/response

👉 **Use case:** Simple applications and quick testing

---

## 🔹 Approach 2: With LangChain

In this approach, I used **LangChain as a wrapper** over Google models.

### Key Points:

* Used `ChatGoogleGenerativeAI`
* Authentication via environment variables
* Structured interaction using messages
* Standardized interface for LLMs

👉 **Use case:** Scalable applications (chatbots, agents, workflows)

---

## ⚖️ SDK vs LangChain Comparison

| Feature     | SDK    | LangChain        |
| ----------- | ------ | ---------------- |
| Control     | High   | Moderate         |
| Memory      | ❌      | ✔                |
| Agents      | ❌      | ✔                |
| RAG Support | Manual | Built-in support |

---

## 💡 Key Learning

* SDK provides **direct control** over the model
* LangChain provides **abstraction and scalability**
* Choosing between them depends on use case

---

## 🎤 Interview Explanation

👉 **Short Answer:**
“I explored both direct SDK usage and LangChain integration with Gemini models to understand the differences in control, flexibility, and scalability.”

👉 **Detailed Answer:**
“In this lesson, I used Google AI Studio to experiment with Gemini models. I first interacted using the SDK for direct control, and then used LangChain to understand how structured messaging and abstraction improve scalability for real-world applications.”

---

## 🚀 Next Steps

* Add memory to chatbot
* Build RAG pipeline
* Integrate with real-world datasets

---

## 📁 Files

* `gemini_sdk_vs_langchain.ipynb` → Implementation notebook
