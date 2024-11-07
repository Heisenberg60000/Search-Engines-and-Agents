# Search-Engines-and-Agents
Explore several search engines:SerperDev,SerpApi and Tavily. Use Langgraph to build search agents.


# LangGraph Agent Builder: Multi-Tool Search and Analysis Framework

LangGraph Agent Builder provides a framework to design intelligent agents capable of complex interactions using search engines, analysis tools, and memory systems. This repository demonstrates how to build state-based agents that can seamlessly switch between tools, retrieve data, and analyze information from multiple sources, creating highly interactive and modular workflows.

## Key Features

- **Agent Workflow with StateGraph**: Create agent workflows that dynamically navigate between tools and states based on the task requirements.
- **Multi-Tool Integration**: Incorporate diverse search engines and tools, enabling agents to explore and analyze information across multiple platforms.
- **Memory and State Management**: Track agent interactions and use memory to enable smooth, context-aware responses.
- **Visualizable Workflows**: Automatically visualize agent states and transitions to aid in debugging and design.

## Getting Started

### Prerequisites
- LangGraph and relevant API keys for search tools
- utilities uploads necessary libraries and reads necessary API keys
- User needs to creata a file chatgpt_api_credentials.yml with the following keys
- openai_api_key: 
- tavily_api_key: 
- serpapi_api_key: 
- serper_api_key: 
### Installation

```bash
pip install langgraph
```

### Usage

The following example demonstrates how to define an agent that toggles between states based on tool requirements and utilizes search tools to fetch and analyze data.

1. **Define the Agent’s State**: Use a `State` dictionary to manage the agent’s input and messages.

    ```python
    from typing_extensions import TypedDict, Annotated

    class State(TypedDict):
        input: str
        messages: Annotated[list, add_messages]
    ```

2. **Set Up Conditional Logic**: Define logic for determining whether to proceed with tool calls or conclude the workflow.

    ```python
    def should_continue(state: State):
        messages = state["messages"]
        last_message = messages[-1]
        if last_message.tool_calls:
            print("Using tools")
            return "tools"
        return END
    ```

3. **Model and Tool Invocation**: Implement functions for model calls and tool interactions.

    ```python
    def call_model(state: State):
        messages = state["messages"]
        response = model_with_tools.invoke(messages)
        return {"messages": [response]}
    ```

4. **Build the StateGraph**: Define and connect nodes to manage transitions between the agent and tool states.

    ```python
    builder = StateGraph(State)

    builder.add_node("agent", call_model)
    builder.add_node("tools", tool_node)
    builder.set_entry_point("agent")
    builder.add_conditional_edges("agent", should_continue, ["tools", END])
    builder.add_edge("tools", "agent")
    ```

5. **Enable Memory and Compile the Workflow**: Utilize memory to retain state information, then compile and visualize the workflow.

    ```python
    # Set up memory
    memory = MemorySaver()
    graph = builder.compile(checkpointer=memory)
    display(Image(graph.get_graph().draw_mermaid_png()))
    ```

This setup creates an agent capable of seamlessly switching between querying tools and analyzing responses, making it suitable for complex, multi-step search and analysis tasks.

## Contributing

Contributions are welcome! Please see the [CONTRIBUTING.md](CONTRIBUTING.md) file for details.

---

### Keywords

- Explore Different Search Engines
- LangGraph
- Multi-Tool Agents
- StateGraph and Memory Management
- Intelligent Workflow Builder

--- 

## Acknowledgments

Special thanks to the open-source community and contributors who have made LangGraph a versatile tool for building advanced search agents.

---

This README outlines how to use LangGraph to build intelligent agents that interact with multiple tools and retain context across sessions, enabling dynamic, search-enabled workflows. Enjoy exploring LangGraph's powerful capabilities!
