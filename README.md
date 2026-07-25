# Building AI Agent

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
[![Kaggle](https://img.shields.io/badge/Kaggle-Open%20Notebook-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/code/lucalullo/building-ai-agent)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

A progressive educational project that shows how to build an AI agent step by step, from scratch, using pure Python.

The project does not use an external LLM or an agent framework. Each version introduces one new architectural concept, making routing, planning, parsing, tool execution and memory easier to understand.

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
    Memory
      ↓
    Output
```

The Planner creates an intermediate execution plan before parsing and execution.

Example:

```python
{
    "tool": "addition",
    "reason": "Add two numbers.",
    "needs_arguments": ["a", "b"]
}
```

## Project versions

| Version | Main concept | Folder |
|---|---|---|
| Version 1 | Base Agent | [`v01-agent-base`](v01-agent-base/) |
| Version 2 | Dedicated Executor | [`v02-executor`](v02-executor/) |
| Version 3 | Generic Parser | [`v03-generic-parser`](v03-generic-parser/) |
| Version 4 | Planner | [`v04-planner`](v04-planner/) |

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

### Version 4 architecture

![Version 4 Planner architecture](v04-planner/Version%204.png)

Open the folder:

[`v04-planner`](v04-planner/)

## Component responsibilities

| Component | Responsibility |
|---|---|
| Router | Selects the most appropriate tool |
| Planner | Creates the execution plan |
| Generic Parser | Extracts the required arguments |
| Executor | Validates the arguments and executes the tool |
| Memory | Stores the complete interaction state |
| Agent | Coordinates the complete workflow |

## Documentation

The repository includes a complete general report in Italian and English:

- [Project report - English](project-report-en.pdf)
- [Relazione del progetto - Italiano](project-report-it.pdf)

Each version folder also contains:

- a Jupyter Notebook;
- an Italian technical report;
- an English technical report;
- an architecture diagram.

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

The notebooks use Python's standard library, so no external agent framework is required.

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

For example, to study the Planner:

```text
v04-planner/building-ai-agent.ipynb
```

## Development approach

The project follows one main principle:

> **One version, one concept.**

Instead of immediately creating a complex system, the architecture evolves through small and understandable steps.

Each completed version remains available as an independent learning resource.

## Roadmap

- [x] Base Agent
- [x] Dedicated Executor
- [x] Generic Parser
- [x] Planner
- [ ] Memory Manager
- [ ] Tool Registry
- [ ] LLM-based routing and parsing
- [ ] Multi-step planning and execution
- [ ] Python package structure

## Current limitations

The current version is intentionally simple:

- the Router is rule-based;
- the Planner is rule-based;
- the Parser uses manual extraction rules;
- memory is still a simple list;
- there is no Tool Registry yet;
- no LLM is used;
- the agent executes one action at a time.

These limitations will be addressed progressively in future versions.

## License

This project is distributed under the [MIT License](LICENSE).

## Author

Created by [Luca Lullo](https://github.com/lucalullo).
