# Building AI Agent

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
[![Kaggle](https://img.shields.io/badge/Kaggle-Open%20Notebook-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/code/lucalullo/building-ai-agent)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

A progressive educational project that shows how to build an AI agent step by step, from scratch, using pure Python.

The current versions do not use an external LLM or an agent framework. Each version introduces one new architectural concept, making routing, planning, parsing, tool execution and memory easier to understand before LLM-based components are introduced.

## Project roadmap

The complete project evolves through five main stages:

1. core agent;
2. modular execution;
3. planning and managed memory;
4. scalable intelligence with a Tool Registry and LLM integration;
5. multi-step execution and professional project structure.

Each stage introduces one architectural improvement while preserving the concepts developed in the previous versions.

![Building AI Agent project roadmap infographic](infographic.png)

## Online notebook

An executable version of the project is available on Kaggle:

### [Open Building AI Agent on Kaggle](https://www.kaggle.com/code/lucalullo/building-ai-agent)

## Current capabilities

The current agent can:

- receive a natural-language request;
- identify the user's intent;
- select the most appropriate tool;
- create a simple execution plan;
- explain why the selected tool should be used;
- determine which arguments are required;
- extract and validate the arguments;
- execute the selected tool;
- handle unsupported requests safely;
- manage memory through dedicated functions;
- centralize tool functions and metadata through a Tool Registry;
- store the request, plan, action, arguments and result in memory.

The included tools support greetings and basic arithmetic operations:

- addition;
- subtraction;
- multiplication;
- division;
- exponentiation.

## Current architecture

```text
User Request
      ↓
    Router
      ↓
   Planner
      ↓
Generic Parser
      ↓
   Executor
      ↓
Memory Manager
      ↓
    Memory
      ↓
    Output
```

The Planner creates an intermediate execution plan before parsing and execution.

The Memory Manager saves, reads and clears memory through dedicated functions.

The Tool Registry provides a central source of tool functions and metadata for the Router, Planner, Generic Parser and Executor.

Example:

```python
{
    "tool": "addition",
    "reason": "Add two numbers.",
    "needs_arguments": ["a", "b"]
}
```

## Project versions

| Version | Main concept | Status | Folder |
|---|---|---|---|
| Version 1 | Base Agent | Completed | [`v01-agent-base`](v01-agent-base/) |
| Version 2 | Dedicated Executor | Completed | [`v02-executor`](v02-executor/) |
| Version 3 | Generic Parser | Completed | [`v03-generic-parser`](v03-generic-parser/) |
| Version 4 | Planner | Completed | [`v04-planner`](v04-planner/) |
| Version 5 | Memory Manager | Completed | [`v05-memory-manager`](v05-memory-manager/) |
| Version 6 | Tool Registry | Completed | [`v06-tool-registry`](v06-tool-registry/) |
| Version 7 | LLM Router | Planned | - |
| Version 8 | LLM Parser | Planned | - |
| Version 9 | Multi-step Agent | Planned | - |
| Version 10 | Professional Project Structure | Planned | - |

## Version 1 - Base Agent

The first version introduces the complete basic agent flow:

```text
User Request
      ↓
    Router
      ↓
    Parser
      ↓
Tool Execution
      ↓
    Memory
```

In this version, the Agent directly coordinates the components and executes the selected tool.

Open the folder:

[`v01-agent-base`](v01-agent-base/)

## Version 2 - Executor

The second version introduces a dedicated Executor component:

```text
User Request
      ↓
    Router
      ↓
    Parser
      ↓
   Executor
      ↓
    Memory
```

Tool validation and execution are separated from the Agent, improving the separation of responsibilities.

Open the folder:

[`v02-executor`](v02-executor/)

## Version 3 - Generic Parser

The third version introduces a generic parsing mechanism based on the parameters required by each tool.

This reduces tool-specific conditions inside the Parser and prepares the architecture for:

- new tools;
- reusable parameter extractors;
- more flexible parsing;
- future LLM integration.

Open the folder:

[`v03-generic-parser`](v03-generic-parser/)

## Version 4 - Planner

The fourth version introduces a dedicated Planner component.

The agent no longer moves directly from tool selection to parsing and execution. It first creates a simple execution plan describing:

- the selected tool;
- the reason for selecting it;
- the arguments required by the tool.

The complete flow becomes:

```text
User Request
      ↓
    Router
      ↓
   Planner
      ↓
Generic Parser
      ↓
   Executor
      ↓
    Memory
      ↓
    Output
```

The Planner is still rule-based and uses the existing Router to identify the most appropriate tool.

If no suitable tool is available, the Planner creates a plan with no selected tool:

```python
{
    "tool": None,
    "reason": "No suitable tool was found for the request.",
    "needs_arguments": []
}
```

The Agent then returns a safe fallback response:

```text
I cannot handle this request yet.
```

The agent state now also includes the execution plan:

```python
{
    "request": "What is 2 + 3?",
    "plan": {
        "tool": "addition",
        "reason": "Add two numbers.",
        "needs_arguments": ["a", "b"]
    },
    "action": "addition",
    "arguments": {
        "a": 2,
        "b": 3
    },
    "result": 5
}
```

This makes the internal behavior of the agent more transparent and prepares the architecture for more advanced planning and multi-step execution.

Open the folder:

[`v04-planner`](v04-planner/)

## Version 5 - Memory Manager

The fifth version introduces a dedicated Memory Manager component.

The Agent no longer writes directly to the memory list. Instead, it delegates memory operations to dedicated functions:

- `save_to_memory(state)`;
- `get_memory()`;
- `get_last_interaction()`;
- `clear_memory()`.

The complete flow becomes:

```text
User Request
      ↓
    Router
      ↓
   Planner
      ↓
Generic Parser
      ↓
   Executor
      ↓
Memory Manager
      ↓
    Memory
      ↓
    Output
```

The Memory Manager is still simple and uses a Python list, but memory operations are now separated from the Agent coordination logic.

Open the folder:

[`v05-memory-manager`](v05-memory-manager/)

## Version 6 - Tool Registry

The sixth version introduces a centralized Tool Registry.

Tool functions and their metadata are no longer distributed across separate structures. Each tool is registered together with:

- the tool function;
- the required parameters;
- a short description;
- the synonyms used by the rule-based Router.

The Tool Registry acts as a shared source of information for the existing components:

- the Router reads registered synonyms;
- the Planner reads tool descriptions and required parameters;
- the Generic Parser reads the parameters required by the selected tool;
- the Executor retrieves and executes the registered function.

The main execution flow remains unchanged:

```text
User Request
     ↓
    Router
     ↓
   Planner
     ↓
Generic Parser
     ↓
   Executor
     ↓
Memory Manager
     ↓
    Memory
     ↓
    Output
```

The Tool Registry is not an additional execution step. It supports the Router, Planner, Generic Parser and Executor by centralizing tool definitions and metadata.

This version is still fully rule-based and does not use an LLM.

Open the folder:

[`v06-tool-registry`](v06-tool-registry/)

## Component responsibilities

| Component | Responsibility |
|---|---|
| Router | Selects the most appropriate tool |
| Planner | Creates the execution plan |
| Generic Parser | Extracts the required arguments |
| Executor | Validates the arguments and executes the tool |
| Memory Manager | Saves, reads and clears memory |
| Tool Registry | Stores tool functions and metadata in one central structure |
| Memory | Stores the complete interaction state |
| Agent | Coordinates the complete workflow |

## Documentation

The repository includes a complete general report in Italian and English:

- [Project report - English](project-report-en.pdf)
- [Relazione del progetto - Italiano](project-report-it.pdf)

Each completed version folder contains:

- a Jupyter Notebook;
- an Italian technical report;
- an English technical report;
- a version-specific architecture diagram.

The repository also includes a general project roadmap infographic:

- [Building AI Agent roadmap infographic](infographic.png)

## Repository structure

```text
building-ai-agent/
├── v01-agent-base/
│   ├── building-ai-agent.ipynb
│   ├── Relazione Versione 1 - Agent Base.pdf
│   ├── Report Version 1 - Base Agent.pdf
│   └── Version 1.png
│
├── v02-executor/
│   ├── building-ai-agent.ipynb
│   ├── Relazione Versione 2 - Executor.pdf
│   ├── Report Version 2 - Executor.pdf
│   └── Version 2.png
│
├── v03-generic-parser/
│   ├── building-ai-agent.ipynb
│   ├── Relazione Versione 3 - Generic Parser.pdf
│   ├── Report Version 3 - Generic Parser.pdf
│   └── Version 3.png
│
├── v04-planner/
│   ├── building-ai-agent.ipynb
│   ├── Relazione Versione 4 - Planner.pdf
│   ├── Report Version 4 - Planner.pdf
│   └── Version 4.png
│
├── v05-memory-manager/
│   ├── building-ai-agent.ipynb
│   ├── Relazione Versione 5 - Memory Manager.pdf
│   ├── Report Version 5 - Memory Manager.pdf
│   └── Version 5.png
│
├── v06-tool-registry/
│   ├── building-ai-agent.ipynb
│   ├── Relazione Versione 6 - Tool Registry.pdf
│   ├── Report Version 6 - Tool Registry.pdf
│   └── Version 6.png
│
├── infographic.png
├── project-report-en.pdf
├── project-report-it.pdf
├── README.md
├── LICENSE
└── .gitignore
```

## Run locally

### Requirements

- Python 3
- Jupyter Notebook or JupyterLab

The current notebooks use Python's standard library, so no external agent framework is required.

Clone the repository:

```bash
git clone https://github.com/lucalullo/building-ai-agent.git
cd building-ai-agent
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Then open the notebook contained in the version folder you want to study and run the cells in order.

For example, to study the Memory Manager:

```text
v05-memory-manager/building-ai-agent.ipynb
```

## Development approach

The project follows one main principle:

> **One version, one concept, one architectural improvement.**

Instead of immediately creating a complex system, the architecture evolves through small and understandable steps.

Each completed version remains available as an independent learning resource.

## Roadmap

- [x] Base Agent
- [x] Dedicated Executor
- [x] Generic Parser
- [x] Planner
- [x] Memory Manager
- [x] Tool Registry
- [ ] LLM Router
- [ ] LLM Parser
- [ ] Multi-step Agent
- [ ] Professional Project Structure

## Current limitations

The current version is intentionally simple:

- the Router is rule-based;
- the Planner is rule-based;
- the Parser uses manual extraction rules;
- memory is still a simple list managed by the Memory Manager;
- the Tool Registry is still a simple in-memory Python dictionary and tool registration is manual;
- no LLM is used;
- the agent executes one action at a time.

These limitations will be addressed progressively in future versions.

## License

This project is distributed under the [MIT License](LICENSE).

## Author

Created by [Luca Lullo](https://github.com/lucalullo).
