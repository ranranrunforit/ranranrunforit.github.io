---
title: "Interview Preparations - Agent Cheatsheet 1"
excerpt: "If I have only 1 hour right before the interview...😅 <br/><img src='/images/AgentCycle.gif'>"
collection: projects
date: 2025-06-23
category: LLM
description: "Notes from HuggingFace🤗 Agent Course."
tags:
  - Agent
  - Tool 
  - Thought
  - Action
  - Observation
---

#  What is an Agent?

An Agent is: an **AI model capable of reasoning, planning, and interacting with its environment**. 

We call it Agent because it has _agency_, aka it has the ability to interact with the environment.

## Let's go more formal

Here’s a more precise definition:

> An Agent is a system that leverages an AI model to interact with its environment in order to achieve a user-defined objective. It combines reasoning, planning, and the execution of actions (often via external tools) to fulfill tasks.

Think of the Agent as having two main parts:

1. **The Brain (AI Model)**

This is where all the thinking happens. The AI model **handles reasoning and planning**.
It decides **which Actions to take based on the situation**.

2. **The Body (Capabilities and Tools)**

This part represents **everything the Agent is equipped to do**.

The **scope of possible actions** depends on what the agent **has been equipped with**. For example, because humans lack wings, they can't perform the "fly" **Action**, but they can execute **Actions** like "walk", "run" ,"jump", "grab", and so on.

## How does an AI take action on its environment?

LLMs are amazing models, but **they can only generate text**. 

However, if you ask a well-known chat application like HuggingChat or ChatGPT to generate an image, they can! How is that possible?

The answer is that the developers of HuggingChat, ChatGPT and similar apps implemented additional functionality (called **Tools**), that the LLM can use to create images.

## What type of tasks can an Agent do?

An Agent can perform any task we implement via **Tools** to complete **Actions**.

For example, if I write an Agent to act as my personal assistant (like Siri) on my computer, and I ask it to "send an email to my Manager asking to delay today's meeting", I can give it some code to send emails. This will be a new Tool the Agent can use whenever it needs to send an email. We can write it in Python:

```python
def send_message_to(recipient, message):
    """Useful to send an e-mail message to a recipient"""
    ...
```

# What are Tools?

One crucial aspect of AI Agents is their ability to take **actions**. As we saw, this happens through the use of **Tools**.

A **Tool is a function given to the LLM**. This function should fulfill a **clear objective**.

Here are some commonly used tools in AI agents:

| Tool            | Description                                                   |
|----------------|---------------------------------------------------------------|
| Web Search     | Allows the agent to fetch up-to-date information from the internet. |
| Image Generation | Creates images based on text descriptions.                  |
| Retrieval      | Retrieves information from an external source.                |
| API Interface  | Interacts with an external API (GitHub, YouTube, Spotify, etc.). |

Those are only examples, as you can in fact create a tool for any use case!

A good tool should be something that **complements the power of an LLM**.

For instance, if you need to perform arithmetic, giving a **calculator tool** to your LLM will provide better results than relying on the native capabilities of the model.

Furthermore, **LLMs predict the completion of a prompt based on their training data**, which means that their internal knowledge only includes events prior to their training. Therefore, if your agent needs up-to-date data you must provide it through some tool.

For instance, if you ask an LLM directly (without a search tool) for today's weather, the LLM will potentially hallucinate random weather.


- A Tool should contain:

  - A **textual description of what the function does**.
  - A *Callable* (something to perform an action).
  - *Arguments* with typings.
  - (Optional) Outputs with typings.

## How do tools work?

LLMs, as we saw, can only receive text inputs and generate text outputs. They have no way to call tools on their own. When we talk about providing tools to an Agent, we mean teaching the LLM about the existence of these tools and instructing it to generate text-based invocations when needed.

For example, if we provide a tool to check the weather at a location from the internet and then ask the LLM about the weather in Paris, the LLM will recognize that this is an opportunity to use the “weather” tool. Instead of retrieving the weather data itself, the LLM will generate text that represents a tool call, such as call weather_tool('Paris'). 

The **Agent** then reads this response, identifies that a tool call is required, executes the tool on the LLM’s behalf, and retrieves the actual weather data. 

The Tool-calling steps are typically not shown to the user: the Agent appends them as a new message before passing the updated conversation to the LLM again. The LLM then processes this additional context and generates a natural-sounding response for the user. From the user’s perspective, it appears as if the LLM directly interacted with the tool, but in reality, it was the Agent that handled the entire execution process in the background.

## How do we give tools to an LLM?

The complete answer may seem overwhelming, but we essentially use the system prompt to provide textual descriptions of available tools to the model:

For this to work, we have to be very precise and accurate about:

1. **What the tool does**
2. **What exact inputs it expects**

This is the reason why tool descriptions are usually provided using expressive but precise structures, such as computer languages or JSON. It's not _necessary_ to do it like that, any precise and coherent format would work.

If this seems too theoretical, let's understand it through a concrete example.

We will implement a simplified **calculator** tool that will just multiply two integers. This could be our Python implementation:

```python
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b
```

So our tool is called `calculator`, it **multiplies two integers**, and it requires the following inputs:

- **`a`** (*int*): An integer.
- **`b`** (*int*): An integer.

The output of the tool is another integer number that we can describe like this:
- (*int*): The product of `a` and `b`.

All of these details are important. Let's put them together in a text string that describes our tool for the LLM to understand.

```text
Tool Name: calculator, Description: Multiply two integers., Arguments: a: int, b: int, Outputs: int
```

> **Reminder:** This textual description is *what we want the LLM to know about the tool*.

When we pass the previous string as part of the input to the LLM, the model will recognize it as a tool, and will know what it needs to pass as inputs and what to expect from the output.

If we want to provide additional tools, we must be consistent and always use the same format. This process can be fragile, and we might accidentally overlook some details.

Is there a better way?

### Auto-formatting Tool sections

Our tool was written in Python, and the implementation already provides everything we need:

- A descriptive name of what it does: `calculator`
- A longer description, provided by the function's docstring comment: `Multiply two integers.`
- The inputs and their type: the function clearly expects two `int`s.
- The type of the output.

There's a reason people use programming languages: they are expressive, concise, and precise.

We could provide the Python source code as the _specification_ of the tool for the LLM, but the way the tool is implemented does not matter. All that matters is its name, what it does, the inputs it expects and the output it provides.

We will leverage Python's introspection features to leverage the source code and build a tool description automatically for us. All we need is that the tool implementation uses type hints, docstrings, and sensible function names. We will write some code to extract the relevant portions from the source code.

After we are done, we'll only need to use a Python decorator to indicate that the `calculator` function is a tool:

```python
@tool
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b

print(calculator.to_string())
```

Note the `@tool` decorator before the function definition.

With the implementation we'll see next, we will be able to retrieve the following text automatically from the source code via the `to_string()` function provided by the decorator:

```text
Tool Name: calculator, Description: Multiply two integers., Arguments: a: int, b: int, Outputs: int
```

As you can see, it's the same thing we wrote manually before!

### Generic Tool implementation

We create a generic `Tool` class that we can reuse whenever we need to use a tool.

> **Disclaimer:** This example implementation is fictional but closely resembles real implementations in most libraries.

```python
from typing import Callable


class Tool:
    """
    A class representing a reusable piece of code (Tool).

    Attributes:
        name (str): Name of the tool.
        description (str): A textual description of what the tool does.
        func (callable): The function this tool wraps.
        arguments (list): A list of argument.
        outputs (str or list): The return type(s) of the wrapped function.
    """
    def __init__(self,
                 name: str,
                 description: str,
                 func: Callable,
                 arguments: list,
                 outputs: str):
        self.name = name
        self.description = description
        self.func = func
        self.arguments = arguments
        self.outputs = outputs

    def to_string(self) -> str:
        """
        Return a string representation of the tool,
        including its name, description, arguments, and outputs.
        """
        args_str = ", ".join([
            f"{arg_name}: {arg_type}" for arg_name, arg_type in self.arguments
        ])

        return (
            f"Tool Name: {self.name},"
            f" Description: {self.description},"
            f" Arguments: {args_str},"
            f" Outputs: {self.outputs}"
        )

    def __call__(self, *args, **kwargs):
        """
        Invoke the underlying function (callable) with provided arguments.
        """
        return self.func(*args, **kwargs)
```

It may seem complicated, but if we go slowly through it we can see what it does. We define a **`Tool`** class that includes:

- **`name`** (*str*): The name of the tool.
- **`description`** (*str*): A brief description of what the tool does.
- **`function`** (*callable*): The function the tool executes.
- **`arguments`** (*list*): The expected input parameters.
- **`outputs`** (*str* or *list*): The expected outputs of the tool.
- **`__call__()`**: Calls the function when the tool instance is invoked.
- **`to_string()`**: Converts the tool's attributes into a textual representation.

We could create a Tool with this class using code like the following:

```python
calculator_tool = Tool(
    "calculator",                   # name
    "Multiply two integers.",       # description
    calculator,                     # function to call
    [("a", "int"), ("b", "int")],   # inputs (names and types)
    "int",                          # output
)
```

But we can also use Python's `inspect` module to retrieve all the information for us! This is what the `@tool` decorator does.

> If you are interested, you can disclose the following section to look at the decorator implementation.

<details>
<summary> decorator code</summary>

```python
import inspect

def tool(func):
    """
    A decorator that creates a Tool instance from the given function.
    """
    # Get the function signature
    signature = inspect.signature(func)

    # Extract (param_name, param_annotation) pairs for inputs
    arguments = []

    for param in signature.parameters.values():
        annotation_name = (
            param.annotation.__name__
            if hasattr(param.annotation, '__name__')
            else str(param.annotation)
        )
        arguments.append((param.name, annotation_name))

    # Determine the return annotation
    return_annotation = signature.return_annotation
    if return_annotation is inspect._empty:
        outputs = "No return annotation"
    else:
        outputs = (
            return_annotation.__name__
            if hasattr(return_annotation, '__name__')
            else str(return_annotation)
        )

    # Use the function's docstring as the description (default if None)
    description = func.__doc__ or "No description provided."

    # The function name becomes the Tool name
    name = func.__name__

    # Return a new Tool instance
    return Tool(
        name=name,
        description=description,
        func=func,
        arguments=arguments,
        outputs=outputs
    )
```

</details>

Just to reiterate, with this decorator in place we can implement our tool like this:

```python
@tool
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b

print(calculator.to_string())
```

And we can use the `Tool`'s `to_string` method to automatically retrieve a text suitable to be used as a tool description for an LLM:

```text
Tool Name: calculator, Description: Multiply two integers., Arguments: a: int, b: int, Outputs: int
```

The description is **injected** in the system prompt. Taking the example with which we started this section, here is how it would look like after replacing the `tools_description`:


### Model Context Protocol (MCP): a unified tool interface

Model Context Protocol (MCP) is an **open protocol** that standardizes how applications **provide tools to LLMs**.
MCP provides:

- A growing list of pre-built integrations that your LLM can directly plug into
- The flexibility to switch between LLM providers and vendors
- Best practices for securing your data within your infrastructure

This means that **any framework implementing MCP can leverage tools defined within the protocol**, eliminating the need to reimplement the same tool interface for each framework.

---

Tools play a crucial role in enhancing the capabilities of AI agents.

To summarize, we learned:

- *What Tools Are*: Functions that give LLMs extra capabilities, such as performing calculations or accessing external data.

- *How to Define a Tool*: By providing a clear textual description, inputs, outputs, and a callable function.

- *Why Tools Are Essential*: They enable Agents to overcome the limitations of static model training, handle real-time tasks, and perform specialized actions.

Now, we can move on to the Agent Workflow where you’ll see how an Agent observes, thinks, and acts. This **brings together everything we’ve covered so far** and sets the stage for creating your own fully functional AI Agent.


# Understanding AI Agents through the Thought-Action-Observation Cycle

In the previous sections, we learned:

- **How tools are made available to the agent in the system prompt**.
- **How AI agents are systems that can 'reason', plan, and interact with their environment**.

In this section, **we’ll explore the complete AI Agent Workflow**, a cycle we defined as Thought-Action-Observation. 

And then, we’ll dive deeper on each of these steps.

## The Core Components

Agents work in a continuous cycle of: **thinking (Thought) → acting (Act) and observing (Observe)**.

Let’s break down these actions together:

1. **Thought**: The LLM part of the Agent decides what the next step should be.
2. **Action:** The agent takes an action, by calling the tools with the associated arguments.
3. **Observation:** The model reflects on the response from the tool.

## The Thought-Action-Observation Cycle

The three components work together in a continuous loop. To use an analogy from programming, the agent uses a **while loop**: the loop continues until the objective of the agent has been fulfilled.

Visually, it looks like this:

<img src="/images/AgentCycle.gif" alt="Think, Act, Observe cycle"/>

In many Agent frameworks, **the rules and guidelines are embedded directly into the system prompt**, ensuring that every cycle adheres to a defined logic.

We see here that in the System Message we defined :

- The *Agent's behavior*.
- The *Tools our Agent has access to*, as we described in the previous section.
- The *Thought-Action-Observation Cycle*, that we bake into the LLM instructions.

Let’s take a small example to understand the process before going deeper into each step of the process.

In the example:

- **Agents iterate through a loop until the objective is fulfilled:**
    
**Alfred’s process is cyclical**. It starts with a thought, then acts by calling a tool, and finally observes the outcome. If the observation had indicated an error or incomplete data, Alfred could have re-entered the cycle to correct its approach.
    
- **Tool Integration:**

The ability to call a tool (like a weather API) enables Alfred to go **beyond static knowledge and retrieve real-time data**, an essential aspect of many AI Agents.

- **Dynamic Adaptation:**

Each cycle allows the agent to incorporate fresh information (observations) into its reasoning (thought), ensuring that the final answer is well-informed and accurate.
    
This example showcases the core concept behind the *ReAct cycle* (a concept we're going to develop in the next section): **the interplay of Thought, Action, and Observation empowers AI agents to solve complex tasks iteratively**. 

By understanding and applying these principles, you can design agents that not only reason about their tasks but also **effectively utilize external tools to complete them**, all while continuously refining their output based on environmental feedback.

---

Let’s now dive deeper into the Thought, Action, Observation as the individual steps of the process.

# Thought: Internal Reasoning and the ReAct Approach
 
>In this section, we dive into the inner workings of an AI agent—its ability to reason and plan. We’ll explore how the agent leverages its internal dialogue to analyze information, break down complex problems into manageable steps, and decide what action to take next. Additionally, we introduce the ReAct approach, a prompting technique that encourages the model to think “step by step” before acting. 


Thoughts represent the **Agent's internal reasoning and planning processes** to solve the task.

This utilises the agent's Large Language Model (LLM) capacity **to analyze information when presented in its prompt**.

Think of it as the agent's internal dialogue, where it considers the task at hand and strategizes its approach.

The Agent's thoughts are responsible for accessing current observations and decide what the next action(s) should be.

Through this process, the agent can **break down complex problems into smaller, more manageable steps**, reflect on past experiences, and continuously adjust its plans based on new information.

Here are some examples of common thoughts:

| Type of Thought | Example |
|----------------|---------|
| Planning | "I need to break this task into three steps: 1) gather data, 2) analyze trends, 3) generate report" |
| Analysis | "Based on the error message, the issue appears to be with the database connection parameters" |
| Decision Making | "Given the user's budget constraints, I should recommend the mid-tier option" |
| Problem Solving | "To optimize this code, I should first profile it to identify bottlenecks" |
| Memory Integration | "The user mentioned their preference for Python earlier, so I'll provide examples in Python" |
| Self-Reflection | "My last approach didn't work well, I should try a different strategy" |
| Goal Setting | "To complete this task, I need to first establish the acceptance criteria" |
| Prioritization | "The security vulnerability should be addressed before adding new features" |

> **Note:** In the case of LLMs fine-tuned for function-calling, the thought process is optional.
> *In case you're not familiar with function-calling, there will be more details in the Actions section.*

## The ReAct Approach

A key method is the **ReAct approach**, which is the concatenation of  "Reasoning" (Think) with "Acting" (Act). 

ReAct is a simple prompting technique that appends "Let's think step by step" before letting the LLM decode the next tokens. 

Indeed, prompting the model to think "step by step" encourages the decoding process toward next tokens **that generate a plan**, rather than a final solution, since the model is encouraged to **decompose** the problem into *sub-tasks*.

This allows the model to consider sub-steps in more detail, which in general leads to less errors than trying to generate the final solution directly.


>We have recently seen a lot of interest for reasoning strategies. This is what's behind models like Deepseek R1 or OpenAI's o1, which have been fine-tuned to "think before answering".
>
>These models have been trained to always include specific _thinking_ sections (enclosed between `<think>` and `</think>` special tokens). This is not just a prompting technique like ReAct, but a training method where the model learns to generate these sections after analyzing thousands of examples that show what we expect it to do.


--- 
Now that we better understand the Thought process, let's go deeper on the second part of the process: Act.

# Actions:  Enabling the Agent to Engage with Its Environment

> In this section, we explore the concrete steps an AI agent takes to interact with its environment. 
>
> We’ll cover how actions are represented (using JSON or code), the importance of the stop and parse approach, and introduce different types of agents.

Actions are the concrete steps an **AI agent takes to interact with its environment**. 

Whether it’s browsing the web for information or controlling a physical device, each action is a deliberate operation executed by the agent. 

For example, an agent assisting with customer service might retrieve customer data, offer support articles, or transfer issues to a human representative.

## Types of Agent Actions

There are multiple types of Agents that take actions differently:

| Type of Agent          | Description                                                                                      |
|------------------------|--------------------------------------------------------------------------------------------------|
| JSON Agent             | The Action to take is specified in JSON format.                                                  |
| Code Agent             | The Agent writes a code block that is interpreted externally.                                    |
| Function-calling Agent | It is a subcategory of the JSON Agent which has been fine-tuned to generate a new message for each action. |

Actions themselves can serve many purposes:

| Type of Action           | Description                                                                              |
|--------------------------|------------------------------------------------------------------------------------------|
| Information Gathering    | Performing web searches, querying databases, or retrieving documents.                    |
| Tool Usage               | Making API calls, running calculations, and executing code.                              |
| Environment Interaction  | Manipulating digital interfaces or controlling physical devices.                         |
| Communication            | Engaging with users via chat or collaborating with other agents.                         |

The LLM only handles text and uses it to describe the action it wants to take and the parameters to supply to the tool. For an agent to work properly, the LLM must STOP generating new tokens after emitting all the tokens to define a complete Action. This passes control from the LLM back to the agent and ensures the result is parseable - whether the intended format is JSON, code, or function-calling. 


## The Stop and Parse Approach

One key method for implementing actions is the **stop and parse approach**. This method ensures that the agent’s output is structured and predictable:

1. **Generation in a Structured Format**:

The agent outputs its intended action in a clear, predetermined format (JSON or code).

2. **Halting Further Generation**:

Once the text defining the action has been emitted, **the LLM stops generating additional tokens**. This prevents extra or erroneous output.

3. **Parsing the Output**:

An external parser reads the formatted action, determines which Tool to call, and extracts the required parameters.

For example, an agent needing to check the weather might output:


```json
Thought: I need to check the current weather for New York.
Action :
{
  "action": "get_weather",
  "action_input": {"location": "New York"}
}
```
The framework can then easily parse the name of the function to call and the arguments to apply.

This clear, machine-readable format minimizes errors and enables external tools to accurately process the agent’s command.

Note: Function-calling agents operate similarly by structuring each action so that a designated function is invoked with the correct arguments.
We'll dive deeper into those types of Agents in a future Unit.

## Code Agents

An alternative approach is using *Code Agents*.
The idea is: **instead of outputting a simple JSON object**, a Code Agent generates an **executable code block—typically in a high-level language like Python**. 

This approach offers several advantages:

- **Expressiveness:** Code can naturally represent complex logic, including loops, conditionals, and nested functions, providing greater flexibility than JSON.
- **Modularity and Reusability:** Generated code can include functions and modules that are reusable across different actions or tasks.
- **Enhanced Debuggability:** With a well-defined programming syntax, code errors are often easier to detect and correct.
- **Direct Integration:** Code Agents can integrate directly with external libraries and APIs, enabling more complex operations such as data processing or real-time decision making.

You must keep in mind that executing LLM-generated code may pose security risks, from prompt injection to the execution of harmful code.
That's why it's recommended to use AI agent frameworks like `smolagents` that integrate default safeguards.

For example, a Code Agent tasked with fetching the weather might generate the following Python snippet:

```python
# Code Agent Example: Retrieve Weather Information
def get_weather(city):
    import requests
    api_url = f"https://api.weather.com/v1/location/{city}?apiKey=YOUR_API_KEY"
    response = requests.get(api_url)
    if response.status_code == 200:
        data = response.json()
        return data.get("weather", "No weather information available")
    else:
        return "Error: Unable to fetch weather data."

# Execute the function and prepare the final answer
result = get_weather("New York")
final_answer = f"The current weather in New York is: {result}"
print(final_answer)
```

In this example, the Code Agent:

- Retrieves weather data **via an API call**,
- Processes the response,
- And uses the print() function to output a final answer.

This method **also follows the stop and parse approach** by clearly delimiting the code block and signaling when execution is complete (here, by printing the final_answer).

---

We learned that Actions bridge an agent's internal reasoning and its real-world interactions by executing clear, structured tasks—whether through JSON, code, or function calls.

This deliberate execution ensures that each action is precise and ready for external processing via the stop and parse approach. In the next section, we will explore Observations to see how agents capture and integrate feedback from their environment.

After this, we will **finally be ready to build our first Agent!**

# Observe: Integrating Feedback to Reflect and Adapt

Observations are **how an Agent perceives the consequences of its actions**.

They provide crucial information that fuels the Agent's thought process and guides future actions.

They are **signals from the environment**—whether it’s data from an API, error messages, or system logs—that guide the next cycle of thought.

In the observation phase, the agent:

- **Collects Feedback:** Receives data or confirmation that its action was successful (or not).
- **Appends Results:** Integrates the new information into its existing context, effectively updating its memory.
- **Adapts its Strategy:** Uses this updated context to refine subsequent thoughts and actions.

For example, if a weather API returns the data *"partly cloudy, 15°C, 60% humidity"*, this observation is appended to the agent’s memory (at the end of the prompt).

The Agent then uses it to decide whether additional information is needed or if it’s ready to provide a final answer.

This **iterative incorporation of feedback ensures the agent remains dynamically aligned with its goals**, constantly learning and adjusting based on real-world outcomes.

These observations **can take many forms**, from reading webpage text to monitoring a robot arm's position. This can be seen like Tool "logs" that provide textual feedback of the Action execution.

| Type of Observation | Example                                                                   |
|---------------------|---------------------------------------------------------------------------|
| System Feedback     | Error messages, success notifications, status codes                       |
| Data Changes        | Database updates, file system modifications, state changes                |
| Environmental Data  | Sensor readings, system metrics, resource usage                           |
| Response Analysis   | API responses, query results, computation outputs                         |
| Time-based Events   | Deadlines reached, scheduled tasks completed                              |

## How Are the Results Appended?

After performing an action, the framework follows these steps in order:

1. **Parse the action** to identify the function(s) to call and the argument(s) to use.  
2. **Execute the action.**  
3. **Append the result** as an **Observation**.  

---
We've now learned the Agent's Thought-Action-Observation Cycle. 

If some aspects still seem a bit blurry, don't worry—we'll revisit and deepen these concepts in future Units. 

Now, it's time to put your knowledge into practice by coding your very first Agent!



# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[HuggingFace🤗 Agent Course](https://huggingface.co/learn/agents-course/unit1/what-are-agents)
