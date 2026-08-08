# PromptTemplate with Different LLM Providers

One of the biggest advantages of **LangChain's `PromptTemplate`** is that it is **provider-independent**.

This means the same prompt template can be used with different LLM providers such as Google Gemini, OpenAI GPT, Anthropic Claude, Hugging Face models, Vertex AI, etc.

Only the **LLM initialization** changes. The `PromptTemplate` remains exactly the same.

---

# Architecture

```text
                 PromptTemplate
                        │
                        ▼
               Formatted Prompt
                        │
        ┌───────────────┼────────────────┬─────────────────┐
        │               │                │                 │
        ▼               ▼                ▼                 ▼
ChatGoogle       ChatOpenAI      ChatAnthropic    ChatHuggingFace
GenerativeAI
```

LangChain formats the prompt once and sends it to whichever LLM you choose.

---

# Example PromptTemplate

```python
from langchain_core.prompts import PromptTemplate

prompt = PromptTemplate.from_template(
    "Explain {topic} in simple language."
)

formatted_prompt = prompt.invoke({
    "topic": "Transformers"
})
```

The output is a formatted prompt.

```text
Explain Transformers in simple language.
```

---

# Using Google Gemini

```python
response = gemini_model.invoke(formatted_prompt)
```

---

# Using OpenAI GPT

```python
response = gpt_model.invoke(formatted_prompt)
```

---

# Using Anthropic Claude

```python
response = claude_model.invoke(formatted_prompt)
```

---

# Using Hugging Face

```python
response = hf_model.invoke(formatted_prompt)
```

---

# What Changes?

Only the model initialization.

## Google

```python
llm = ChatGoogleGenerativeAI(...)
```

## OpenAI

```python
llm = ChatOpenAI(...)
```

## Anthropic

```python
llm = ChatAnthropic(...)
```

## Hugging Face

```python
llm = ChatHuggingFace(...)
```

---

# What Remains the Same?

- PromptTemplate
- Template structure
- Input variables
- `prompt.invoke()`
- `llm.invoke()`

---

# Why Companies Use PromptTemplate

Companies rarely hardcode prompts.

Instead, they create reusable templates and inject dynamic values at runtime.

Example:

```python
prompt = PromptTemplate.from_template(
    "Generate a product description for {product}."
)
```

Runtime:

```python
{"product": "iPhone 16"}
```

```python
{"product": "Samsung S25"}
```

```python
{"product": "OnePlus 14"}
```

The prompt is written once and reused for thousands or millions of requests.

---

# Benefits in Production

- Reusable prompts
- Easy maintenance
- Dynamic input injection
- Consistent prompt structure
- Scalable across millions of requests
- Provider-independent

---

# PromptTemplate Workflow

```text
Create PromptTemplate
          │
          ▼
Provide Input Variables
          │
          ▼
Formatted Prompt
          │
          ▼
Selected LLM
          │
          ▼
AI Response
```

---

# Important Note

`PromptTemplate` **does not fetch live data**.

It only creates a formatted prompt.

If a prompt requires live information such as:

- Amazon products
- Weather
- Stock prices
- Database records

the application must first retrieve that data using APIs, databases, SQL, or RAG, and then inject it into the prompt.

Example:

```python
PromptTemplate.from_template(
"""
You are a shopping assistant.

Products:
{products}

Question:
{question}
"""
)
```

Here:

- `{products}` → comes from an API or database.
- `{question}` → comes from the user.

The LLM only generates a response using the provided data.

---

# Interview Answer

**Q. Does `PromptTemplate` change when using different LLM providers?**

**Answer:**

No. `PromptTemplate` is a LangChain abstraction and is provider-independent. It is responsible for creating reusable prompts by replacing placeholders with runtime values. The same `PromptTemplate` can be used with Google Gemini, OpenAI GPT, Anthropic Claude, Hugging Face models, Vertex AI, and other LangChain-supported providers. Only the LLM initialization changes; the prompt template remains the same.

---

# Key Takeaway ⭐

Remember this simple rule:

```text
PromptTemplate
        │
        ▼
Creates the Prompt
        │
        ▼
Any LangChain Chat Model

ChatGoogleGenerativeAI
ChatOpenAI
ChatAnthropic
ChatHuggingFace
ChatVertexAI
```

**PromptTemplate belongs to LangChain, not to any specific LLM provider.**
