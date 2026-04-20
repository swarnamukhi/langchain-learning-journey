# 📘 Lesson 1: Vertex AI (Direct SDK Usage)

## 📌 Overview

In this lesson, I explored how to use **Google Cloud Vertex AI** to interact with **Gemini models** using the official Python SDK.

Unlike Google AI Studio (used in Lesson 0), Vertex AI is a **production-level service** that requires authentication, project configuration, and billing setup.

---

## 🎯 Objective

* Understand how to use Gemini models via Vertex AI
* Learn Google Cloud authentication process
* Configure project and billing (quota project)
* Generate responses using Vertex AI SDK

---

## 🧠 Topics Covered

* Google Cloud authentication (`gcloud auth`)
* Project configuration
* Quota project (billing setup)
* Vertex AI initialization
* Gemini model usage (`GenerativeModel`)

---

## ⚙️ Setup & Authentication

### 🔹 Install required package

```python
!pip install google-cloud-aiplatform
```

---

### 🔹 Authenticate Google Cloud

```bash
gcloud auth application-default login
```

👉 Opens browser → login with Google account

---

### 🔹 Set project

```bash
gcloud config set project YOUR_PROJECT_ID
```

---

### 🔹 Set quota project (for billing)

```bash
gcloud auth application-default set-quota-project YOUR_PROJECT_ID
```

---

## 🚀 Implementation

### 🔹 Initialize Vertex AI

```python
import vertexai
from vertexai.generative_models import GenerativeModel

vertexai.init(
    project="YOUR_PROJECT_ID",
    location="us-central1"
)
```

---

### 🔹 Load Gemini model

```python
model = GenerativeModel("gemini-2.5-flash-lite")
```

---

### 🔹 Generate response

```python
response = model.generate_content(
    "Explain Generative AI in simple terms"
)

print(response.text)
```

---

## 🔄 Flow of Execution

1. Install SDK
2. Authenticate using gcloud
3. Set project & quota
4. Initialize Vertex AI
5. Load Gemini model
6. Send prompt
7. Receive response

---

## 💡 Key Learning

* Vertex AI requires proper authentication and billing setup
* Gemini models can be accessed via SDK in production environments
* Direct SDK provides full control over model interaction

---

## ⚖️ Vertex AI vs Google AI Studio

| Feature        | Google AI Studio | Vertex AI              |
| -------------- | ---------------- | ---------------------- |
| Setup          | Simple           | Requires configuration |
| Authentication | API Key          | GCP Auth               |
| Use Case       | Testing          | Production             |
| Control        | Moderate         | High                   |

---

## 🎤 Interview Explanation

👉 **Short Answer:**
“I used Vertex AI SDK to interact with Gemini models by setting up Google Cloud authentication, configuring the project, and generating responses programmatically.”

👉 **Detailed Answer:**
“In this lesson, I worked with Google Cloud Vertex AI, which is a production-level service. I configured authentication using gcloud, set up the project and billing, and used the GenerativeModel class to interact with Gemini models. This helped me understand how GenAI applications are built in real-world cloud environments.”

---

## 🚀 Next Steps

* Integrate Vertex AI with LangChain
* Add memory for conversational AI
* Build a chatbot interface

---

## 📁 Files

* `vertexai_basic.ipynb` → Implementation notebook

