---
title: "Interview Preparations - MCP Cheatsheet"
excerpt: "When I have only 1 hour right before the interview...😅 <br/><img src='/images/1.png'>"
collection: projects
date: 2025-06-28
category: LLM
description: "Notes from HuggingFace🤗 MCP course."
tags:
  - MCP
  - Host 
  - Client
  - Server
  - Tool
---


# The Fundamentals of Model Context Protocol (MCP)

# Key Concepts and Terminology

Before diving deeper into the Model Context Protocol, it's important to understand the key concepts and terminology that form the foundation of MCP. This section will introduce the fundamental ideas that underpin the protocol and provide a common vocabulary for discussing MCP implementations throughout the course.

MCP is often described as the "USB-C for AI applications." Just as USB-C provides a standardized physical and logical interface for connecting various peripherals to computing devices, MCP offers a consistent protocol for linking AI models to external capabilities. This standardization benefits the entire ecosystem:

- **users** enjoy simpler and more consistent experiences across AI applications
- **AI application developers** gain easy integration with a growing ecosystem of tools and data sources
- **tool and data providers** need only create a single implementation that works with multiple AI applications
- the broader ecosystem benefits from increased interoperability, innovation, and reduced fragmentation

## The Integration Problem

The **M×N Integration Problem** refers to the challenge of connecting M different AI applications to N different external tools or data sources without a standardized approach. 

### Without MCP (M×N Problem)

Without a protocol like MCP, developers would need to create M×N custom integrations—one for each possible pairing of an AI application with an external capability. 

![Without MCP](/images/1.png)

Each AI application would need to integrate with each tool/data source individually. This is a very complex and expensive process which introduces a lot of friction for developers, and high maintenance costs.

Once we have multiple models and multiple tools, the number of integrations becomes too large to manage, each with its own unique interface.

![Multiple Models and Tools](/images/1a.png)

### With MCP (M+N Solution)

MCP transforms this into an M+N problem by providing a standard interface: each AI application implements the client side of MCP once, and each tool/data source implements the server side once. This dramatically reduces integration complexity and maintenance burden.

![With MCP](/images/2.png)

Each AI application implements the client side of MCP once, and each tool/data source implements the server side once.

## Core MCP Terminology

Now that we understand the problem that MCP solves, let's dive into the core terminology and concepts that make up the MCP protocol.

<Tip>

MCP is a standard like HTTP or USB-C, and is a protocol for connecting AI applications to external tools and data sources. Therefore, using standard terminology is crucial to making the MCP work effectively. 

When documenting our applications and communicating with the community, we should use the following terminology.

</Tip>

### Components

Just like client server relationships in HTTP, MCP has a client and a server.

![MCP Components](/images/3.png)

- **Host**: The user-facing AI application that end-users interact with directly. Examples include Anthropic's Claude Desktop, AI-enhanced IDEs like Cursor, inference libraries like Hugging Face Python SDK, or custom applications built in libraries like LangChain or smolagents. Hosts initiate connections to MCP Servers and orchestrate the overall flow between user requests, LLM processing, and external tools.

- **Client**: A component within the host application that manages communication with a specific MCP Server. Each Client maintains a 1:1 connection with a single Server, handling the protocol-level details of MCP communication and acting as an intermediary between the Host's logic and the external Server.

- **Server**: An external program or service that exposes capabilities (Tools, Resources, Prompts) via the MCP protocol.

<Tip warning={true}>

A lot of content uses 'Client' and 'Host' interchangeably. Technically speaking, the host is the user-facing application, and the client is the component within the host application that manages communication with a specific MCP Server.

</Tip>

### Capabilities

Of course, your application's value is the sum of the capabilities it offers. So the capabilities are the most important part of your application. MCP's can connect with any software service, but there are some common capabilities that are used for many AI applications.

| Capability | Description | Example |
| ---------- | ----------- | ------- |
| **Tools** | Executable functions that the AI model can invoke to perform actions or retrieve computed data. Typically relating to the use case of the application. | A tool for a weather application might be a function that returns the weather in a specific location. |
| **Resources** | Read-only data sources that provide context without significant computation. | A researcher assistant might have a resource for scientific papers. |
| **Prompts** | Pre-defined templates or workflows that guide interactions between users, AI models, and the available capabilities. | A summarization prompt. |
| **Sampling** | Server-initiated requests for the Client/Host to perform LLM interactions, enabling recursive actions where the LLM can review generated content and make further decisions. | A writing application reviewing its own output and decides to refine it further. |

In the following diagram, we can see the collective capabilities applied to a use case for a code agent.

![collective diagram](/images/8.png)

This application might use their MCP entities in the following way:

| Entity | Name | Description |
| --- | --- | --- |
| Tool | Code Interpreter | A tool that can execute code that the LLM writes. |
| Resource | Documentation | A resource that contains the documentation of the application. |
| Prompt | Code Style | A prompt that guides the LLM to generate code. |
| Sampling | Code Review | A sampling that allows the LLM to review the code and make further decisions. |

### Conclusion

Understanding these key concepts and terminology provides the foundation for working with MCP effectively. In the following sections, we'll build on this foundation to explore the architectural components, communication protocol, and capabilities that make up the Model Context Protocol. 


# Architectural Components of MCP

In the previous section, we discussed the key concepts and terminology of MCP. Now, let's dive deeper into the architectural components that make up the MCP ecosystem.

## Host, Client, and Server

The Model Context Protocol (MCP) is built on a client-server architecture that enables structured communication between AI models and external systems. 

![MCP Architecture](/images/4.png)

The MCP architecture consists of three primary components, each with well-defined roles and responsibilities: Host, Client, and Server. We touched on these in the previous section, but let's dive deeper into each component and their responsibilities.

### Host

The **Host** is the user-facing AI application that end-users interact with directly. 

Examples include:
- AI Chat apps like OpenAI ChatGPT or Anthropic's Claude Desktop
- AI-enhanced IDEs like Cursor, or integrations to tools like Continue.dev
- Custom AI agents and applications built in libraries like LangChain or smolagents

The Host's responsibilities include:
- Managing user interactions and permissions
- Initiating connections to MCP Servers via MCP Clients
- Orchestrating the overall flow between user requests, LLM processing, and external tools
- Rendering results back to users in a coherent format

In most cases, users will select their host application based on their needs and preferences. For example, a developer may choose Cursor for its powerful code editing capabilities, while domain experts may use custom applications built in smolagents.

### Client

The **Client** is a component within the Host application that manages communication with a specific MCP Server. Key characteristics include:

- Each Client maintains a 1:1 connection with a single Server
- Handles the protocol-level details of MCP communication
- Acts as the intermediary between the Host's logic and the external Server

### Server

The **Server** is an external program or service that exposes capabilities to AI models via the MCP protocol. Servers:

- Provide access to specific external tools, data sources, or services
- Act as lightweight wrappers around existing functionality
- Can run locally (on the same machine as the Host) or remotely (over a network)
- Expose their capabilities in a standardized format that Clients can discover and use

## Communication Flow

Let's examine how these components interact in a typical MCP workflow:

<Tip>

In the next section, we'll dive deeper into the communication protocol that enables these components with practical examples.

</Tip>

1. **User Interaction**: The user interacts with the **Host** application, expressing an intent or query.

2. **Host Processing**: The **Host** processes the user's input, potentially using an LLM to understand the request and determine which external capabilities might be needed.

3. **Client Connection**: The **Host** directs its **Client** component to connect to the appropriate Server(s).

4. **Capability Discovery**: The **Client** queries the **Server** to discover what capabilities (Tools, Resources, Prompts) it offers.

5. **Capability Invocation**: Based on the user's needs or the LLM's determination, the Host instructs the **Client** to invoke specific capabilities from the **Server**.

6. **Server Execution**: The **Server** executes the requested functionality and returns results to the **Client**.

7. **Result Integration**: The **Client** relays these results back to the **Host**, which incorporates them into the context for the LLM or presents them directly to the user.

A key advantage of this architecture is its modularity. A single **Host** can connect to multiple **Servers** simultaneously via different **Clients**. New **Servers** can be added to the ecosystem without requiring changes to existing **Hosts**. Capabilities can be easily composed across different **Servers**.

<Tip>

As we discussed in the previous section, this modularity transforms the traditional M×N integration problem (M AI applications connecting to N tools/services) into a more manageable M+N problem, where each Host and Server needs to implement the MCP standard only once.

</Tip>

The architecture might appear simple, but its power lies in the standardization of the communication protocol and the clear separation of responsibilities between components. This design allows for a cohesive ecosystem where AI models can seamlessly connect with an ever-growing array of external tools and data sources.

## Conclusion

These interaction patterns are guided by several key principles that shape the design and evolution of MCP. The protocol emphasizes **standardization** by providing a universal protocol for AI connectivity, while maintaining **simplicity** by keeping the core protocol straightforward yet enabling advanced features. **Safety** is prioritized by requiring explicit user approval for sensitive operations, and discoverability enables dynamic discovery of capabilities. The protocol is built with **extensibility** in mind, supporting evolution through versioning and capability negotiation, and ensures **interoperability** across different implementations and environments.

In the next section, we'll explore the communication protocol that enables these components to work together effectively.


# The Communication Protocol

MCP defines a standardized communication protocol that enables Clients and Servers to exchange messages in a consistent, predictable way. This standardization is critical for interoperability across the community. In this section, we'll explore the protocol structure and transport mechanisms used in MCP.

![MCP Protocol](/images/9.png)

## JSON-RPC: The Foundation

At its core, MCP uses **JSON-RPC 2.0** as the message format for all communication between Clients and Servers. JSON-RPC is a lightweight remote procedure call protocol encoded in JSON, which makes it:

- Human-readable and easy to debug
- Language-agnostic, supporting implementation in any programming environment
- Well-established, with clear specifications and widespread adoption

![message types](/images/5.png)

The protocol defines three types of messages:

### 1. Requests

Sent from Client to Server to initiate an operation. A Request message includes:
- A unique identifier (`id`)
- The method name to invoke (e.g., `tools/call`)
- Parameters for the method (if any)

Example Request:

```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "weather",
    "arguments": {
      "location": "San Francisco"
    }
  }
}
```

### 2. Responses

Sent from Server to Client in reply to a Request. A Response message includes:
- The same `id` as the corresponding Request
- Either a `result` (for success) or an `error` (for failure)

Example Success Response:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "result": {
    "temperature": 62,
    "conditions": "Partly cloudy"
  }
}
```

Example Error Response:
```json
{
  "jsonrpc": "2.0",
  "id": 1,
  "error": {
    "code": -32602,
    "message": "Invalid location parameter"
  }
}
```

### 3. Notifications

One-way messages that don't require a response. Typically sent from Server to Client to provide updates or notifications about events.

Example Notification:
```json
{
  "jsonrpc": "2.0",
  "method": "progress",
  "params": {
    "message": "Processing data...",
    "percent": 50
  }
}
```

## Transport Mechanisms

JSON-RPC defines the message format, but MCP also specifies how these messages are transported between Clients and Servers. Two primary transport mechanisms are supported:

### stdio (Standard Input/Output)

The stdio transport is used for local communication, where the Client and Server run on the same machine:

The Host application launches the Server as a subprocess and communicates with it by writing to its standard input (stdin) and reading from its standard output (stdout).

<Tip>

**Use cases** for this transport are local tools like file system access or running local scripts.

</Tip>

The main **Advantages** of this transport are that it's simple, no network configuration required, and securely sandboxed by the operating system.

### HTTP + SSE (Server-Sent Events) / Streamable HTTP

The HTTP+SSE transport is used for remote communication, where the Client and Server might be on different machines:

Communication happens over HTTP, with the Server using Server-Sent Events (SSE) to push updates to the Client over a persistent connection.

<Tip>

**Use cases** for this transport are connecting to remote APIs, cloud services, or shared resources.

</Tip>

The main **Advantages** of this transport are that it works across networks, enables integration with web services, and is compatible with serverless environments.

Recent updates to the MCP standard have introduced or refined "Streamable HTTP," which offers more flexibility by allowing servers to dynamically upgrade to SSE for streaming when needed, while maintaining compatibility with serverless environments.

## The Interaction Lifecycle

In the previous section, we discussed the lifecycle of a single interaction between a Client (💻) and a Server (🌐). Let's now look at the lifecycle of a complete interaction between a Client and a Server in the context of the MCP protocol.

The MCP protocol defines a structured interaction lifecycle between Clients and Servers:

### Initialization

The Client connects to the Server and they exchange protocol versions and capabilities, and the Server responds with its supported protocol version and capabilities.

<table style="width: 100%;">
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">→<br>initialize</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">←<br>response</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">→<br>initialized</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
</table>

The Client confirms the initialization is complete via a notification message.

### Discovery

The Client requests information about available capabilities and the Server responds with a list of available tools.

<table style="width: 100%;">
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">→<br>tools/list</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">←<br>response</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
</table>

This process could be repeated for each tool, resource, or prompt type.

### Execution

The Client invokes capabilities based on the Host's needs.

<table style="width: 100%;">
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">→<br>tools/call</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">←<br>notification (optional progress)</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">←<br>response</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
</table>

### Termination

The connection is gracefully closed when no longer needed and the Server acknowledges the shutdown request.

<table style="width: 100%;">
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">→<br>shutdown</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">←<br>response</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
  <tr>
    <td style="background-color: lightgreen; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">💻</td>
    <td style="text-align: center;">→<br>exit</td>
    <td style="background-color: lightblue; text-align: center; padding: 10px; border: 1px solid #ccc; border-radius: 4px;">🌐</td>
  </tr>
</table>

The Client sends the final exit message to complete the termination.

## Protocol Evolution

The MCP protocol is designed to be extensible and adaptable. The initialization phase includes version negotiation, allowing for backward compatibility as the protocol evolves. Additionally, capability discovery enables Clients to adapt to the specific features each Server offers, enabling a mix of basic and advanced Servers in the same ecosystem.

# Understanding MCP Capabilities

MCP Servers expose a variety of capabilities to Clients through the communication protocol. These capabilities fall into four main categories, each with distinct characteristics and use cases. Let's explore these core primitives that form the foundation of MCP's functionality.

<Tip>

In this section, we'll show examples as framework agnostic functions in each language. This is to focus on the concepts and how they work together, rather than the complexities of any framework.

In the coming units, we'll show how these concepts are implemented in MCP specific code.

</Tip>

## Tools

Tools are executable functions or actions that the AI model can invoke through the MCP protocol.

- **Control**: Tools are typically **model-controlled**, meaning that the AI model (LLM) decides when to call them based on the user's request and context.
- **Safety**: Due to their ability to perform actions with side effects, tool execution can be dangerous. Therefore, they typically require explicit user approval.
- **Use Cases**: Sending messages, creating tickets, querying APIs, performing calculations.

**Example**: A weather tool that fetches current weather data for a given location:

<hfoptions id="tool-example">
<hfoption id="python">

```python
def get_weather(location: str) -> dict:
    """Get the current weather for a specified location."""
    # Connect to weather API and fetch data
    return {
        "temperature": 72,
        "conditions": "Sunny",
        "humidity": 45
    }
```

</hfoption>
<hfoption id="javascript">

```javascript
function getWeather(location) {
    // Connect to weather API and fetch data
    return {
        temperature: 72,
        conditions: 'Sunny',
        humidity: 45
    };
}
```

</hfoption>
</hfoptions>

## Resources

Resources provide read-only access to data sources, allowing the AI model to retrieve context without executing complex logic.

- **Control**: Resources are **application-controlled**, meaning the Host application typically decides when to access them.
- **Nature**: They are designed for data retrieval with minimal computation, similar to GET endpoints in REST APIs.
- **Safety**: Since they are read-only, they typically present lower security risks than Tools.
- **Use Cases**: Accessing file contents, retrieving database records, reading configuration information.

**Example**: A resource that provides access to file contents:

<hfoptions id="resource-example">
<hfoption id="python">

```python
def read_file(file_path: str) -> str:
    """Read the contents of a file at the specified path."""
    with open(file_path, 'r') as f:
        return f.read()
```

</hfoption>
<hfoption id="javascript">

```javascript
function readFile(filePath) {
    // Using fs.readFile to read file contents
    const fs = require('fs');
    return new Promise((resolve, reject) => {
        fs.readFile(filePath, 'utf8', (err, data) => {
            if (err) {
                reject(err);
                return;
            }
            resolve(data);
        });
    });
}
```

</hfoption>
</hfoptions>

## Prompts

Prompts are predefined templates or workflows that guide the interaction between the user, the AI model, and the Server's capabilities.

- **Control**: Prompts are **user-controlled**, often presented as options in the Host application's UI.
- **Purpose**: They structure interactions for optimal use of available Tools and Resources.
- **Selection**: Users typically select a prompt before the AI model begins processing, setting context for the interaction.
- **Use Cases**: Common workflows, specialized task templates, guided interactions.

**Example**: A prompt template for generating a code review:

<hfoptions id="prompt-example">
<hfoption id="python">

```python
def code_review(code: str, language: str) -> list:
    """Generate a code review for the provided code snippet."""
    return [
        {
            "role": "system",
            "content": f"You are a code reviewer examining {language} code. Provide a detailed review highlighting best practices, potential issues, and suggestions for improvement."
        },
        {
            "role": "user",
            "content": f"Please review this {language} code:\n\n```{language}\n{code}\n```"
        }
    ]
```

</hfoption>
<hfoption id="javascript">

```javascript
function codeReview(code, language) {
    return [
        {
            role: 'system',
            content: `You are a code reviewer examining ${language} code. Provide a detailed review highlighting best practices, potential issues, and suggestions for improvement.`
        },
        {
            role: 'user',
            content: `Please review this ${language} code:\n\n\`\`\`${language}\n${code}\n\`\`\``
        }
    ];
}
```

</hfoption>
</hfoptions>

## Sampling

Sampling allows Servers to request the Client (specifically, the Host application) to perform LLM interactions.

- **Control**: Sampling is **server-initiated** but requires Client/Host facilitation.
- **Purpose**: It enables server-driven agentic behaviors and potentially recursive or multi-step interactions.
- **Safety**: Like Tools, sampling operations typically require user approval.
- **Use Cases**: Complex multi-step tasks, autonomous agent workflows, interactive processes.

**Example**: A Server might request the Client to analyze data it has processed:

<hfoptions id="sampling-example">
<hfoption id="python">

```python
def request_sampling(messages, system_prompt=None, include_context="none"):
    """Request LLM sampling from the client."""
    # In a real implementation, this would send a request to the client
    return {
        "role": "assistant",
        "content": "Analysis of the provided data..."
    }
```

</hfoption>
<hfoption id="javascript">

```javascript
function requestSampling(messages, systemPrompt = null, includeContext = 'none') {
    // In a real implementation, this would send a request to the client
    return {
        role: 'assistant',
        content: 'Analysis of the provided data...'
    };
}

function handleSamplingRequest(request) {
    const { messages, systemPrompt, includeContext } = request;
    // In a real implementation, this would process the request and return a response
    return {
        role: 'assistant',
        content: 'Response to the sampling request...'
    };
}
```

</hfoption>
</hfoptions>

The sampling flow follows these steps:
1. Server sends a `sampling/createMessage` request to the client
2. Client reviews the request and can modify it
3. Client samples from an LLM
4. Client reviews the completion
5. Client returns the result to the server

<Tip>

This human-in-the-loop design ensures users maintain control over what the LLM sees and generates. When implementing sampling, it's important to provide clear, well-structured prompts and include relevant context.

</Tip>

## How Capabilities Work Together

Let's look at how these capabilities work together to enable complex interactions. In the table below, we've outlined the capabilities, who controls them, the direction of control, and some other details.

| Capability | Controlled By | Direction | Side Effects | Approval Needed | Typical Use Cases |
|------------|---------------|-----------|--------------|-----------------|-------------------|
| Tools      | Model (LLM)   | Client → Server | Yes (potentially) | Yes | Actions, API calls, data manipulation |
| Resources  | Application   | Client → Server | No (read-only) | Typically no | Data retrieval, context gathering |
| Prompts    | User          | Server → Client | No | No (selected by user) | Guided workflows, specialized templates |
| Sampling   | Server        | Server → Client → Server | Indirectly | Yes | Multi-step tasks, agentic behaviors |

These capabilities are designed to work together in complementary ways:

1. A user might select a **Prompt** to start a specialized workflow
2. The Prompt might include context from **Resources**
3. During processing, the AI model might call **Tools** to perform specific actions
4. For complex operations, the Server might use **Sampling** to request additional LLM processing

The distinction between these primitives provides a clear structure for MCP interactions, enabling AI models to access information, perform actions, and engage in complex workflows while maintaining appropriate control boundaries.

## Discovery Process

One of MCP's key features is dynamic capability discovery. When a Client connects to a Server, it can query the available Tools, Resources, and Prompts through specific list methods:

- `tools/list`: Discover available Tools
- `resources/list`: Discover available Resources
- `prompts/list`: Discover available Prompts

This dynamic discovery mechanism allows Clients to adapt to the specific capabilities each Server offers without requiring hardcoded knowledge of the Server's functionality.

## Conclusion

Understanding these core primitives is essential for working with MCP effectively. By providing distinct types of capabilities with clear control boundaries, MCP enables powerful interactions between AI models and external systems while maintaining appropriate safety and control mechanisms.

In the next section, we'll explore how Gradio integrates with MCP to provide easy-to-use interfaces for these capabilities. 

# Acknowledgements

This overview was created with the help of deep and manual research, drawing inspiration and information from the excellent resource:

[HuggingFace🤗 MCP Course](https://huggingface.co/learn/mcp-course/unit1/introduction)
