# 📘 Lesson 3: LangChain Memory + Session Handling (Conversational AI)

## 📌 Overview

In this lesson, I built a **stateful conversational AI system** using LangChain and Gemini models.

Unlike previous lessons where each request was independent, here I implemented **memory**, allowing the model to remember previous conversations and respond contextually.

---

## 🎯 Objective

* Understand how to maintain conversation history
* Implement session-based memory
* Build a chatbot-like experience
* Learn how LangChain manages memory internally

---

## 🧠 Core Concept

👉 This project introduces:

> **Stateful LLM Interaction**

Instead of:

```
Input → Model → Output
```

Now:

```
Conversation History + Input → Model → Contextual Output
```

---

## ⚙️ Libraries and Classes Used

### 🔹 1. ChatGoogleGenerativeAI

```python
from langchain_google_genai import ChatGoogleGenerativeAI
```

* Connects LangChain to Gemini models
* Handles API communication

---

### 🔹 2. RunnableWithMessageHistory

```python
from langchain_core.runnables.history import RunnableWithMessageHistory
```

👉 This is the **most important class in this project**

* Wraps the model
* Automatically manages conversation history
* Adds previous messages to each new request

---

### 🔹 3. InMemoryChatMessageHistory

```python
from langchain_core.chat_history import InMemoryChatMessageHistory
```

* Stores conversation in memory
* Acts like a temporary database
* Stores:

  * User messages
  * AI responses

---

## 🏗️ Step-by-Step Implementation

---

### 🔹 Step 1: Initialize Model

```python
model = ChatGoogleGenerativeAI(
    model="gemini-2.5-flash-lite",
    google_api_key="your_api_key"
)
```

👉 This creates the LLM instance

---

### 🔹 Step 2: Create Memory Store

```python
store = {}
```

👉 This dictionary:

* Stores session-wise chat history
* Key = session_id
* Value = conversation history

---

### 🔹 Step 3: Define Session Function

```python
def get_session_history(session_id):
    if session_id not in store:
        store[session_id] = InMemoryChatMessageHistory()
    return store[session_id]
```

👉 What happens here:

* Checks if session exists
* If not → creates new memory
* Returns memory object

💡 This enables **multi-user support**

---

### 🔹 Step 4: Create Chain with Memory

```python
chain = RunnableWithMessageHistory(model, get_session_history)
```

👉 This is the key step:

* Combines:

  * Model
  * Memory
* Automatically injects conversation history

---

### 🔹 Step 5: Define Session ID

```python
session_id = "1209"
```

👉 Represents a unique user/session

---

### 🔹 Step 6: Invoke Chain

```python
response1 = chain.invoke(
    "Hi",
    config={"configurable": {"session_id": session_id}}
)
```

👉 Important:

* `session_id` tells:
  👉 which memory to use

---

## 🔄 Full Flow (VERY IMPORTANT)

```
User Input
   ↓
Chain receives input
   ↓
Fetch session history using session_id
   ↓
Combine:
   - Previous messages
   - Current input
   ↓
Send to LLM
   ↓
Generate response
   ↓
Store response in memory
```

---

## 💬 Example Conversation Flow

### First Message:

```
User: Hi
AI: Hello! How can I help you?
```

---

### Second Message:

```
User: Suggest places for travelling in summer
AI: Suggests destinations
```

---

### Third Message:

```
User: Which country is cheapest?
```

👉 Model understands context:

* Travel topic
* Continues conversation naturally

---

## 🔥 Key Learning

### 🔹 1. Stateless vs Stateful

| Type      | Behavior               |
| --------- | ---------------------- |
| Stateless | No memory              |
| Stateful  | Remembers conversation |

---

### 🔹 2. Role of session_id

* Tracks user
* Enables multi-user conversations
* Maintains separate histories

---

### 🔹 3. Memory Handling

* Stored in Python dictionary
* Temporary (resets when program ends)

---

### 🔹 4. RunnableWithMessageHistory

* Automatically manages memory
* Simplifies implementation
* Core for chatbot systems

---

## ⚠️ Limitations

* Memory is temporary (in-memory only)
* Not persistent across sessions
* Not scalable for production

---

## 🚀 Future Improvements

* Use database (Redis, MongoDB) for memory
* Add long-term memory
* Build full chatbot UI

---

👉 Short Answer:

“I implemented session-based memory using RunnableWithMessageHistory, which allows the model to maintain conversation history and provide context-aware responses.”

---

👉 Detailed Answer:

“In this project, I built a conversational AI system using LangChain and Gemini models. I used RunnableWithMessageHistory to manage conversation memory and InMemoryChatMessageHistory to store chat history. By passing a session_id, I enabled session-based interactions where the model remembers previous messages and responds contextually. This helped me understand how real-world chatbots maintain conversation flow.”

---

## 📁 Files

* `langchain_memory_chat.ipynb` → Implementation notebook

