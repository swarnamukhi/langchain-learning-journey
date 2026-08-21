# EDA Agent — ReAct Agent Using LangChain

## 1. Project Overview
In this project, I built an EDA (Exploratory Data Analysis) agent using Python, Pandas, LangChain, and ChatGroq.
The project evolved step by step:
1. First, I implemented EDA operations as normal Python functions.
2. Then, I converted those functions into LangChain tools using the `@tool` decorator.
3. I provided tool descriptions through docstrings so that the LLM could understand the purpose of each tool.
4. I bound the tools to the LLM using `bind_tools()`.
5. I manually implemented the tool-calling flow to understand what happens internally.
6. Finally, I used the LangChain Agent framework to automate the tool-calling loop.

The important learning was understanding the difference between:
- Normal Python functions
- LangChain tools
- LLM tool calling
- Manual tool execution
- Agent-based tool execution
- ReAct-style reasoning and action loops

# 2. EDA Tools Created
The project contains several EDA tools:
- `check_null_values`
- `Duplicate_check`
- `check_unknown`
- `unique_check`
- `categorical_value_counts`
- `check_numerical_summery`
- `check_correlation`
- `check_outliers`
- `check_distribution`
These tools perform operations such as:
- Detecting missing values
- Detecting duplicate rows
- Detecting `unknown` values
- Finding unique categorical values
- Calculating categorical value counts
- Generating numerical statistical summaries
- Calculating correlations
- Detecting potential outliers using IQR
- Generating numerical distribution histograms
# 3. First Stage — Normal Python Functions
Initially, the EDA operations were implemented as normal Python functions.
Example:

```python
def check_null_values():
    """
    Check the dataset for null/missing values.
    Return the number and percentage of null values
    for each column containing null values.
    """
# EDA logic

At this stage, the function is simply a Python function.
The LLM does not automatically know that this function exists.
The function can be executed directly by Python:
result = check_null_values()
There is no agent involved at this stage.
4. Second Stage — Converting Functions into Tools
To make the functions available to the LLM, I used the LangChain @tool decorator.
Example:
from langchain_core.tools import tool

@tool
def check_null_values():
    """
    Check the dataset for null/missing values.
    Return the number and percentage of null values
    for each column containing null values.
    """
    
    # EDA logic
The @tool decorator converts the normal Python function into a LangChain Tool.
This gives LangChain metadata about the tool, including:
•	Tool name 
•	Tool description 
•	Input schema 
•	Function that should be executed 
5. Why Tool Descriptions Are Important
The docstring is important because the LLM needs to understand what each tool does.
For example:
@tool
def check_outliers():
    """
    Check numerical columns for potential outliers.
    Calculate Q1, Q3, IQR, lower bound, and upper bound.
    Return the outlier count and percentage for each column.
    """
The LLM can use this description to understand:
"This tool is useful when the user asks about outliers."
Therefore, good tool descriptions help the LLM select the appropriate tool.
The docstring does not execute the function.
It provides information about the tool to the model.
6. Tools Created for the LLM
The tools are collected into a list:
tools = [
    check_null_values,
    Duplicate_check,
    check_unknown,
    unique_check,
    check_correlation,
    categorical_value_counts,
    check_numerical_summery,
    check_outliers,
    check_distribution
]
Now these tools can be provided to the LLM.
7. Tool Binding
I used:
model_tool_binded = llm.bind_tools(tools)
Tool binding means:
The LLM is informed about the tools that are available to it and can request those tools when appropriate.
For example, if the user asks:
Check whether my dataset contains null values.
the LLM can decide to call:
check_null_values
The important point is:
bind_tools() does NOT execute the tools.
It only gives the LLM the ability to request tool calls.
8. What Does the LLM Return?
When the LLM receives a question that requires a tool, it can return an AI message containing a tool call.
Conceptually:
User:
"Check the null values."

        ↓

LLM

        ↓

Tool call:
name = check_null_values
arguments = {}
The LLM is saying:
"I want this tool to be executed."
9. Important Distinction — LLM Does Not Execute Python
The LLM does not directly execute the Python function.
The LLM only produces a structured tool call.
For example:
LLM
 ↓
Tool call:
check_null_values({})
Something else must execute that tool.
This is where the tool executor or agent loop comes into the picture.
10. Manual Tool Calling — What I Implemented First
Before using a ReAct agent, I manually implemented the tool-calling workflow to understand how it works internally.
The process was:
User Question
      ↓
LLM
      ↓
LLM generates tool call
      ↓
Read the tool call
      ↓
Execute the selected tool
      ↓
Create ToolMessage
      ↓
Send ToolMessage back to LLM
      ↓
LLM generates final response
This manual implementation helped me understand what an agent framework is actually automating.
11. Manually Executing a Tool
After converting a function into a LangChain tool, it can be invoked using:
response = check_null_values.invoke({})
The {} is the input passed to the tool.
The function does not require any parameters, so an empty dictionary is used.
The important distinction is:
check_null_values()
is the normal Python function style.
Whereas:
check_null_values.invoke({})
is the LangChain Tool invocation style.
12. Tool Input and Tool Output
The direction of communication is:
LLM
 ↓
Tool input
 ↓
Tool
 ↓
Tool output
 ↓
LLM
The tool expects an input from its caller.
The tool then executes and produces a result.
That result can be sent back to the LLM.
13. ToolMessage
After executing the tool manually, the result needs to be represented as a ToolMessage.
The important message sequence is:
HumanMessage
      ↓
AIMessage containing tool_call
      ↓
ToolMessage containing tool result
      ↓
LLM
The order matters.
The ToolMessage must correspond to the tool call generated by the AIMessage.
The tool call ID connects the tool result with the original tool request.
14. Complete Manual Workflow
The complete manual flow is:
                USER
                  |
                  v
             HumanMessage
                  |
                  v
          Tool-bound LLM
                  |
                  v
        AIMessage + tool_call
                  |
                  v
        Identify requested tool
                  |
                  v
        Execute tool manually
                  |
                  v
             Tool result
                  |
                  v
             ToolMessage
                  |
                  v
          Send messages back
             to the LLM
                  |
                  v
          Does LLM need
          another tool?
             /       \
           YES       NO
            |         |
            v         v
       Execute       Final
       another       answer
        tool
            |
            └──────> LLM
This is the core agent loop.
15. Problem with Manual Implementation
Manual implementation is useful for learning, but it becomes complicated when there are many tools.
For example, the code has to handle:
•	Reading the tool call 
•	Identifying the correct tool 
•	Passing arguments 
•	Executing the tool 
•	Capturing the result 
•	Creating the ToolMessage 
•	Sending the result back to the LLM 
•	Checking whether another tool is required 
•	Repeating the process 
•	Detecting when the final answer is ready 
For multiple tools and multiple tool calls, manually managing this loop becomes cumbersome.
16. What Is an Agent?
An agent is a system that combines:
LLM
+
Tools
+
Decision-making
+
Execution loop
The agent allows the LLM to decide what action should be taken based on the user's request and the available tools.
For example:
User:
"Analyze my dataset for missing values and outliers."

                ↓

              Agent

                ↓

      LLM decides what to do

                ↓

       check_null_values()

                ↓

          Tool result

                ↓

      LLM evaluates result

                ↓

       check_outliers()

                ↓

          Tool result

                ↓

        LLM generates
        final response
17. What Is ReAct?
ReAct stands for:
Reason + Act
The basic idea is:
Reason
  ↓
Act
  ↓
Observe
  ↓
Reason
  ↓
Act
  ↓
Observe
  ↓
...
  ↓
Final Answer
In a tool-using agent:
LLM decides what action is required
        ↓
Tool is executed
        ↓
Tool result is observed
        ↓
LLM decides what to do next
For example:
User:
"Find missing values and outliers."

        ↓

Reason:
"I need missing-value information."

        ↓

Act:
check_null_values()

        ↓

Observe:
Tool result

        ↓

Reason:
"I also need outlier information."

        ↓

Act:
check_outliers()

        ↓

Observe:
Tool result

        ↓

Final Answer
18. LangChain Agent
LangChain provides an agent framework that automates the tool-calling loop.
In the current LangChain API, an agent can be created using:
from langchain.agents import create_agent
Conceptually:
agent = create_agent(
    model=llm,
    tools=tools
)
The agent receives:
•	The LLM 
•	The available tools 
and manages the interaction between them.
19. What Does the LangChain Agent Do?
The agent automates the process that I previously implemented manually.
Instead of manually doing:
LLM
 ↓
Read tool call
 ↓
Execute tool
 ↓
Create ToolMessage
 ↓
Send result to LLM
 ↓
Check for another tool
 ↓
Repeat
the agent manages this loop.
Conceptually:
User
 ↓
Agent
 ↓
LLM
 ↓
Tool call
 ↓
Agent executes tool
 ↓
Tool result
 ↓
Agent sends result to LLM
 ↓
LLM decides next action
 ↓
...
 ↓
Final response
20. Manual Implementation vs LangChain Agent
Manual Implementation
In the manual implementation, the developer controls the entire loop.
LLM
 ↓
Developer reads tool call
 ↓
Developer executes tool
 ↓
Developer creates ToolMessage
 ↓
Developer sends result to LLM
 ↓
Developer checks whether another tool is required
Advantages:
•	Excellent for learning 
•	Complete control 
•	Easy to understand internally 
Disadvantages:
•	More code 
•	More message handling 
•	More error handling 
•	Difficult to maintain as tools increase 
•	Developer must implement the loop 
LangChain Agent
With a LangChain agent:
LLM
+
Tools
 ↓
Agent
 ↓
Automatic tool-calling loop
Advantages:
•	Less orchestration code 
•	Automatic tool execution 
•	Handles repeated tool calls 
•	Easier to build agentic applications 
•	Easier to add more tools 
21. The Key Difference
The most important difference is:
Manual approach
I control the tool-calling loop.
Agent approach
The agent framework manages the tool-calling loop and allows the LLM to dynamically decide which tools to use.
22. bind_tools() vs create_agent()
These two concepts should not be confused.
bind_tools()
model_tool_binded = llm.bind_tools(tools)
This gives the LLM knowledge of the available tools and allows it to generate tool calls.
It does not itself provide the complete autonomous execution loop.
Conceptually:
LLM + tool definitions
        ↓
LLM can generate tool calls
create_agent()
agent = create_agent(
    model=llm,
    tools=tools
)
This creates an agent that manages the model/tool interaction loop.
Conceptually:
LLM
 ↓
Tool call
 ↓
Tool execution
 ↓
Tool result
 ↓
LLM
 ↓
Another tool?
 ↓
Final answer
23. Why Use an Agent for the EDA Project?
The EDA project has multiple tools:
check_null_values
Duplicate_check
check_unknown
unique_check
categorical_value_counts
check_numerical_summery
check_correlation
check_outliers
check_distribution
The user may ask different questions.
For example:
"Check missing values."
The agent can select:
check_null_values
If the user asks:
"Find the outliers."
the agent can select:
check_outliers
If the user asks:
"Perform a complete EDA."
the agent may need multiple tools.
This is where agentic behavior becomes useful.
24. Why Not Always Use an Agent?
Agents are not always the best choice.
If the workflow is completely fixed:
Check nulls
 ↓
Check duplicates
 ↓
Check unknowns
 ↓
Numerical summary
 ↓
Outliers
 ↓
Correlation
 ↓
Final report
there may be no reason to let an LLM decide the sequence.
A deterministic workflow can be simpler, faster, cheaper, and easier to debug.
Agents are useful when the workflow requires dynamic decision-making.
25. Advantages of an Agent
1. Dynamic tool selection
The LLM can select tools based on the user's request.
2. Multiple tool calls
The agent can use several tools in one request.
3. Dynamic reasoning
The result of one tool can influence what happens next.
4. Less orchestration code
The framework handles the repetitive tool-calling loop.
5. Extensibility
New EDA tools can be added to the available tool set.
26. Limitations of Agents
1. Less deterministic
The LLM controls the decision-making, so execution may vary.
2. More model calls
A complex request may require:
LLM → Tool → LLM → Tool → LLM
This can increase cost.
3. Higher latency
More model/tool interactions can increase response time.
4. Incorrect tool selection is possible
Poor tool descriptions can cause the LLM to select the wrong tool.
5. Debugging is more complicated
A fixed workflow is easier to trace than an autonomous decision-making loop.
27. Why Good Tool Descriptions Matter
The agent's decision-making depends heavily on the information it receives about the tools.
For example:
@tool
def check_outliers():
    """
    Check numerical columns for potential outliers.
    Calculate Q1, Q3, IQR, lower bound, and upper bound.
    Return the outlier count and percentage for each column.
    """
This tells the LLM:
•	What the tool does 
•	What type of data it works with 
•	What information it returns 
•	When it may be useful 
Therefore:
Good tool design + good tool descriptions → better tool selection.
28. LangChain Is Not the Model Provider
LangChain and Groq have different responsibilities.
Groq
 ↓
Provides model inference
while:
LangChain
 ↓
Provides application framework
 ↓
Tools
Agents
Messages
Orchestration
In this project:
             LangChain
                 |
                 v
             Agent
                 |
                 v
             ChatGroq
                 |
                 v
          GPT-OSS model
                 |
                 v
             EDA Tools
The agent architecture is therefore not inherently specific to Groq.
The model provider can be changed while keeping the overall agent concept.
29. Interview Question: What Is a ReAct Agent?
Answer:
A ReAct agent is an agentic pattern where the LLM alternates between reasoning about what action is required and taking that action through tools. The result of the action is then provided back to the LLM, allowing it to decide the next step until it can produce a final answer.
Short version:
ReAct = Reason → Act → Observe → Reason → Act → Final Answer.
30. Interview Question: What Is the Difference Between Tool Calling and an Agent?
Answer:
Tool calling allows an LLM to generate a structured request to call a tool. An agent adds an orchestration loop around the model and tools so that tool calls can be executed, results can be returned to the LLM, and additional actions can be taken until the task is complete.
31. Interview Question: What Does bind_tools() Do?
Answer:
bind_tools() makes the available tools known to the LLM and enables the LLM to generate structured tool calls. It does not by itself implement the complete autonomous tool-execution loop.
32. Interview Question: Why Do We Need @tool?
Answer:
The @tool decorator converts a normal Python function into a LangChain tool. It exposes metadata such as the tool name, description, and input schema so the framework and LLM can understand how the function can be used.
33. Interview Question: Why Are Tool Docstrings Important?
Answer:
Tool descriptions help the LLM understand the purpose, expected inputs, and outputs of each tool. The LLM uses this information when deciding which tool is appropriate for a user request.
34. Interview Question: Does the LLM Execute the Tool?
Answer:
No.
The LLM generates a structured tool call. The agent/runtime executes the actual Python tool and returns the result to the LLM.
35. Interview Question: What Happens After the LLM Generates a Tool Call?
Answer:
The agent identifies the requested tool, executes it with the supplied arguments, creates a tool result message, sends that result back to the LLM, and allows the LLM to decide whether another tool call is necessary or whether it can provide the final answer.
36. Interview Question: Why Use an Agent Instead of a Fixed Workflow?
Answer:
An agent is useful when the required sequence of actions is not known in advance and needs dynamic decision-making. A fixed workflow is preferable when the sequence is deterministic and must be predictable.
37. Interview Question: Is LangChain Agent Specific to Groq?
Answer:
No. LangChain is an application framework and supports integrations with multiple model providers. Groq is the model inference provider used in this project. The agent concept is not inherently tied to Groq.
38. Final Architecture of the EDA Agent
The final conceptual architecture is:
                         USER
                           |
                           v
                    EDA QUESTION
                           |
                           v
                    LANGCHAIN AGENT
                           |
                           v
                          LLM
                           |
             "Which tool should I use?"
                           |
          +----------------+----------------+
          |                |                |
          v                v                v
   Null Check        Outlier Check    Correlation
      Tool               Tool             Tool
          |                |                |
          +----------------+----------------+
                           |
                           v
                     TOOL RESULT
                           |
                           v
                          LLM
                           |
                   Need another tool?
                     /           \
                   YES            NO
                    |              |
                    v              v
                More tools     FINAL ANSWER
39. Overall Learning Journey
The project evolved as follows:
Normal Python Functions
          ↓
EDA Functions
          ↓
@tool
          ↓
LangChain Tools
          ↓
bind_tools()
          ↓
LLM generates tool calls
          ↓
Manual tool execution
          ↓
ToolMessage
          ↓
Manual tool-calling loop
          ↓
Understand Agent Internals
          ↓
LangChain Agent
          ↓
Automated ReAct-style loop
The most important conceptual progression is:
I first understood how the tool-calling loop works manually, and then used LangChain's agent abstraction to automate that loop.
This approach helped me understand that an agent is not magic. At its core, it is an LLM-driven decision loop that selects tools, executes actions, observes results, and continues until it can produce an answer.

