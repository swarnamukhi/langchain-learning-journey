# LangChain: ChatPromptTemplate, StrOutputParser, Chains & Helper Methods

## Topics Covered

- `PromptTemplate` vs `ChatPromptTemplate`
- `from_template()` vs `from_messages()`
- Helper / Factory Methods
- `StrOutputParser`
- `StrOutputParser` vs `display(Markdown())`
- LangChain Chains
- Automatic input/output flow between chain components
- Manual invocation vs Chains

---

# 1. PromptTemplate vs ChatPromptTemplate

The easiest way to remember the difference:

```text
PromptTemplate
      ↓
String Template

ChatPromptTemplate
      ↓
Conversation Template
