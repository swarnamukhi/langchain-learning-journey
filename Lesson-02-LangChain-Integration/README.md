# 📘 Lesson 2: LangChain + Google GenAI (Gemini API) – Detailed Understanding

## 📌 What I Did in This Project

In this lesson, I built a simple Generative AI application using **LangChain** integrated with **Google Gemini models** through the **Google GenAI API**.

The main goal of this project was to understand:

* How to interact with Gemini models using LangChain
* How API-based authentication works
* How to structure prompts using system and user messages
* How to control model behavior using parameters like temperature

---

## 🧠 Step-by-Step What Happens in My Code

### 🔹 Step 1: Install Required Libraries

```python
!pip install langchain langchain-google-genai
```

👉 Here, I install:

* `langchain` → framework to manage LLM interactions
* `langchain-google-genai` → connector between LangChain and Gemini models

---

### 🔹 Step 2: Authentication using API Key

```python
import os
os.environ["GOOGLE_API_KEY"] = "your_api_key"
```

👉 Instead of using Google Cloud authentication, I use an **API key**.

✔ This is simpler
✔ No need for `gcloud auth`
✔ Direct access to Gemini models

---

### 🔹 Step 3: Initialize the Model

```python
from langchain_google_genai import ChatGoogleGenerativeAI

llm = ChatGoogleGenerativeAI(
    model="gemini-2.5-flash-lite"
)
```

👉 What happens here:

* I create a **LangChain wrapper object**
* This object internally connects to Gemini model via API
* This is like saying:
  👉 “Use this AI model for my application”

---

### 🔹 Step 4: Create Structured Messages

```python
from langchain_core.messages import HumanMessage, SystemMessage

messages = [
    SystemMessage(content="You are a helpful assistant"),
    HumanMessage(content="Explain importance of Physical exercise in simple terms")
]
```

👉 This is a very important concept:

* `SystemMessage` → defines AI behavior
* `HumanMessage` → actual user query

💡 Instead of plain text, we use **structured conversation format**

---

### 🔹 Step 5: Configure Model Behavior

```python
model = llm.bind(
    temperature=0,
    max_output_tokens=1000
)
```

👉 Here I control how the model behaves:

* `temperature = 0` → gives **accurate and consistent answers**
* `max_output_tokens` → limits response size

---

### 🔹 Step 6: Invoke the Model

```python
response = model.invoke(messages)
```

👉 What happens internally:

1. LangChain formats messages
2. Sends request to Gemini API
3. Model processes input
4. Returns response

---

### 🔹 Step 7: Display Output

```python
print(response.content)
```

👉 The AI-generated response is displayed

---

## 🔁 Second Example (Creative Use Case)

```python
message2 = [
    SystemMessage(content="You are a helpful assistant"),
    HumanMessage(content="Explain vitamins required for the human body and their importance")
]

creative_model = llm.bind(temperature=0.7, max_output_tokens=1000)
response2 = creative_model.invoke(message2)
```

👉 Difference here:

* `temperature = 0.7`
  → more **creative and detailed output**

💡 Same model → different behavior based on configuration

---

## 🔥 What I Learned (VERY IMPORTANT)

### 1️⃣ API-based Authentication

* No need for Google Cloud setup
* Uses API key for direct access

---

### 2️⃣ Structured Prompting

* System + Human messages improve control
* More scalable than plain prompts

---

### 3️⃣ Model Control

* Temperature changes output style
* Token limit controls length

---

### 4️⃣ Reusability

* Same model can handle multiple tasks
* Just change input + parameters

---

## ⚖️ Important Concept (VERY IMPORTANT)

👉 This project is **NOT Vertex AI**

✔ Uses: Google GenAI API
✔ Authentication: API Key


👉 Detailed Answer:

“In this project, I integrated LangChain with Gemini models using the ChatGoogleGenerativeAI class. I used API key-based authentication instead of Google Cloud authentication, which simplified the setup. I structured prompts using system and human messages and controlled model output using parameters like temperature and token limits. I also experimented with different configurations to understand how model behavior changes.”

---

## 🚀 Final Understanding

This project helped me understand:

* How LLMs are used in applications
* Difference between SDK and API-based access
* How LangChain simplifies LLM interaction

---

