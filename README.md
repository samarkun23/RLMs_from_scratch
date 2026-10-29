
# Recursive Language Models (RLMs)

**Exploring long-context reasoning through programmatic context inspection, iterative execution, and recursive LLM calls.**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/samarkun23/RLMs_from_scratch/blob/main/RLMs.ipynb)

An experimental implementation of an RLM-style architecture built from scratch using Python and the Gemini API.

Instead of passing an entire context directly to an LLM, this implementation allows the model to explore data programmatically through a Python REPL, execute code, inspect intermediate results, and iteratively work toward a final answer.

## Overview

Traditional LLM workflows often rely on placing relevant information directly into a model's context window. This can become inefficient when working with large datasets or long documents.

This project explores an alternative approach: **letting the model write code to inspect and process its context rather than requiring it to read everything at once.**

The LLM interacts with a persistent Python execution environment, observes outputs, and refines its approach over multiple iterations.

## Architecture

```text
User Query + Context
        |
        v
   Root LLM (Gemini)
        |
        v
  Python Code Generation
        |
        v
      RLMRepl
        |
        +----> Inspect / Search / Process Context
        |
        +----> Optional Sub-LLM Calls
        |
        v
   Execution Feedback
        |
        +------> Root LLM (Next Iteration)
        |
        v
     FINAL(answer)
```

## Key Features

- **Iterative Reasoning Loop** — Generates and executes Python code over multiple iterations until a final answer is submitted or the iteration limit is reached.
- **Programmatic Context Inspection** — Searches, filters, slices, and analyzes context using Python.
- **Persistent REPL Environment** — Maintains variables between execution steps within a task.
- **Recursive LLM Calls** — Provides an `llm_query()` interface for delegating subtasks to a secondary LLM call.
- **Execution Feedback** — Sends REPL output back to the root model to guide subsequent iterations.
- **Configurable Execution** — Supports iteration limits, output truncation, and verbose execution logs.
- **Synthetic Dataset Experiments** — Explores programmatic data analysis using generated datasets.

## Tech Stack

- Python
- Google Gemini API
- Google Colab
- Requests
- Python standard library

## Project Structure

```text
RLMs/
├── RLMs.ipynb
└── README.md
```

## Getting Started

### 1. Open the Notebook

Click the **Open in Colab** badge above or open `RLMs.ipynb` in Google Colab.

### 2. Configure the Gemini API Key

Create an API key through [Google AI Studio](https://aistudio.google.com/apikey).

In Google Colab, store the key in **Secrets** under the name `GEMINI_API_KEY` and enable notebook access.

Never hardcode API keys or commit secrets to version control.

### 3. Install Dependencies

```python
!pip install requests
```

### 4. Run the Notebook

Execute the notebook cells in order to initialize the Gemini API integration, generate the dataset, initialize the REPL, and run the RLM loop.

## Example

```python
answer = run_llm(
    query="Count how many Software Engineers are based in Tokyo.",
    context=dataset_text,
    max_iterations=10,
    verbose=True
)

print(answer)
```

The model can inspect the context using generated Python code, evaluate results, and submit its final answer through `FINAL(answer)`.

## Design Principles

### Code as a Context-Exploration Tool

The model uses Python operations to extract relevant information from a larger context, reducing the need to repeatedly include the entire dataset in model prompts.

### Feedback-Driven Execution

Each iteration receives the previous execution output, allowing the model to adjust its next action based on observed results.

### Delegation Through Sub-LLM Calls

The `llm_query()` interface enables optional delegation of subtasks to a secondary LLM call when additional language-model analysis is useful.

## Current Limitations

- The implementation is experimental and may require multiple iterations for complex tasks.
- Model-generated code can fail or produce incorrect results.
- API latency, rate limits, token usage, and costs depend on the selected Gemini model and account.
- Large contexts and execution outputs may require additional memory and truncation strategies.
- The current Python REPL uses `exec()` and is **not a secure sandbox** for untrusted code.
- This is an experimental RLM-style implementation, not a production-ready system.

## Future Improvements

- More robust error recovery and retry handling.
- Improved context chunking and subtask orchestration.
- Token and API usage tracking.
- Structured execution traces.
- Safer, isolated code execution.
- Further experimentation with long-context reasoning workflows.

## Motivation

This project was built to explore how LLMs can interact with large amounts of information through programmatic inspection, iterative execution, and recursive delegation.

The goal is to understand the engineering principles behind RLM-style systems and experiment with alternatives to conventional long-context prompting.

---

**Status:** Experimental / In Development
