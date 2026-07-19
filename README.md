# Building AI Agent

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
[![Kaggle](https://img.shields.io/badge/Kaggle-Open%20Notebook-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/code/lucalullo/building-ai-agent)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

A progressive educational project that shows how to build an AI agent step by step.

The project starts from the fundamental architecture of an agent rather than from an external agent framework. Each version introduces a new concept, making routing, parsing, tool execution and memory easier to understand.

## Online notebook

An executable version of the project is available on Kaggle:

### [Open Building AI Agent on Kaggle](https://www.kaggle.com/code/lucalullo/building-ai-agent)

## Current capabilities

The current agent can:

- receive a natural-language request;
- identify the user's intent;
- select an appropriate tool;
- extract the required arguments;
- validate the arguments;
- execute the selected tool;
- store the interaction state in memory.

The included tools support greetings and basic arithmetic operations such as addition, subtraction, multiplication, division and exponentiation.

## Project versions

| Version | Main concept | Folder |
|---|---|---|
| Version 1 | Base Agent | [`v01-agent-base`](v01-agent-base/) |
| Version 2 | Dedicated Executor | [`v02-executor`](v02-executor/) |
| Version 3 | Generic Parser | [`v03-generic-parser`](v03-generic-parser/) |

## Version 1 - Base Agent

The first version introduces the complete basic agent flow:

```text
User request
      ↓
    Router
      ↓
    Parser
      ↓
Tool execution
      ↓
    Memory
```

In this version, the Agent directly coordinates the components and executes the selected tool.

Open the folder:

[`v01-agent-base`](v01-agent-base/)

## Version 2 - Executor

The second version introduces a dedicated Executor component:

```text
User request
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

## Development approach

The project follows one main principle:

> **One version, one concept.**

Instead of immediately creating a complex system, the architecture evolves through small and understandable steps.

Each completed version remains available as an independent learning resource.

## Roadmap

- [x] Base Agent
- [x] Dedicated Executor
- [x] Generic Parser
- [ ] Planning component
- [ ] Managed memory
- [ ] Tool registry
- [ ] LLM-based routing and parsing
- [ ] Multi-step execution
- [ ] Python package structure

## License

This project is distributed under the [MIT License](LICENSE).

## Author

Created by [Luca Lullo](https://github.com/lucalullo).
