
# Lesson 04 - Continuous Chatbot Memory

## Overview

This lesson demonstrates how to build a continuous conversational chatbot using LangChain with:

- while True loop
- Continuous user interaction
- Session-based memory
- chain.invoke()
- SystemMessage and HumanMessage
- Persistent conversation using session_id

The chatbot keeps running until the user types:

```python
exit
```

---

## Concepts Covered

### 1. Continuous Chatbot

Using:

```python
while True:
```

the chatbot continuously accepts user input instead of running only once.

---

### 2. User Input Handling

```python
user_input = input("You: ")
```

Allows users to dynamically interact with the chatbot.

---

### 3. Session-Based Memory

```python
config={
    "configurable": {
        "session_id": session_id
    }
}
```

The session_id helps maintain conversation history across multiple interactions.

---

### 4. LangChain Message Types

#### SystemMessage

Defines the AI assistant behavior.

```python
SystemMessage(content=system_instruction)
```

#### HumanMessage

Represents user input.

```python
HumanMessage(content=user_input)
```

---

### 5. Chain Invocation

```python
response = chain.invoke(
    [
        SystemMessage(content=system_instruction),
        HumanMessage(content=user_input)
    ],
    config={
        "configurable": {
            "session_id": session_id
        }
    }
)
```

Used to send messages to the LLM while maintaining memory.

---

## Features

- Continuous chatting
- Dynamic user interaction
- Memory persistence
- Session-based conversation
- Exit handling
- Markdown response display

---

## Project Structure

```text
Lesson-04-Continuous-Chatbot-Memory
│
├── Continuous-Chatbot-Memory.ipynb
└── README.md
```

---

## Technologies Used

- Python
- LangChain
- Google Gemini / Vertex AI
- Jupyter Notebook / Google Colab

---

## Learning Outcome

After completing this lesson, you will understand:

- How continuous chat systems work
- How LangChain handles conversational memory
- How session_id maintains context
- Difference between single invocation and continuous interaction
- Real-world chatbot flow using loops and memory

---

## Future Improvements

Possible enhancements:

- Multi-user session handling
- Database-backed memory
- Streamlit chatbot UI
- Long-term memory storage
- RAG integration
- Voice-based chatbot

---

## Author

Swarna Mukhi
