# agents.gasd — Structured Context for Deterministic Agentic Tasks

`agents.gasd` provides high-rigor, structured examples and templates for **General Agentic Software & Design (GASD)**. 

### 💡 The Key Benefit
> **Introducing formal rigor and deterministic constraints for any type of task while still keeping the flexibility of natural language.**

It is inspired by the need for better agent context, taking ideas from projects like [agents.md](https://agents.md/), and evolving them into a formal specification for complex agentic workflows. For the full language definition, visit the [**Official GASD Specification**](https://github.com/General-Agentic-Software-Design/General-Agentic-Software-Design-Language).

## 🚀 The Core Vision

In the world of AI-driven agents, the biggest hurdle is **Ambiguity**. Standard prompts and markdown documentation often leave too much room for interpretation, leading to inconsistent task execution ("Agent Drift").

**GASD (General Agentic Software & Design)** acts as a **Structure Bridge**:

```text
What is needed  →  GASD (Structure Bridge)  →  Executed Outcome
  "Requirement"      "How to execute it"       "Consistent result"
                     + constraints for
                     deterministic behavior
```

## 💎 Why agents.gasd? (The Comparison)

While [agents.md](https://agents.md/) establishes the excellent concept of a "README for agents," `agents.gasd` takes this a step further by introducing formal rigor and deterministic constraints for any type of task **while still keeping the flexibility of natural language**.

| Feature | [agents.md](https://agents.md/) | agents.gasd |
|---|---|---|
| **Format** | Free-form Markdown | Formal EBNF Grammar |
| **Precision** | Informational (Natural Language) | Executable (Keywords: `ACHIEVE`, `ENSURE`) |
| **Readability** | High (Pure Natural Language) | **High (Near-Natural Language syntax)** |
| **Validation** | Manual Review | Structural `@annotations` & Invariants |
| **Traceability** | Indirect/Implicit | Direct `TRACE` & `@trace` directives |
| **Determinism** | High Variance (Agent-dependent) | Low Variance (Constraint-locked) |

By using GASD, you get the **user-friendly readability of natural language** combined with the power of formal specifications. 

### 🌟 The "Best of Both Worlds" Advantage
GASD was designed to be as close to natural language as possible, ensuring that **humans can easily read and write it**, while providing benefits that informal markdown cannot:
- **Zero Ambiguity**: No more "Agent Drift" where different models interpret instructions differently.
- **Built-in Guardrails**: `ENSURE` and `VALIDATE` clauses act as automated quality gates.
- **Strict Data Contracts**: `TYPE` definitions prevent structural errors before they happen.
- **Native Traceability**: Every step is logically linked to its original requirement via `TRACE`.

## 🛠️ Tooling & Support

GASD is backed by a growing ecosystem of tools to ensure your specifications are correct and easy to read:

- [**GASD-Parser-Validator**](https://github.com/General-Agentic-Software-Design/GASD-Parser-Validator): A powerful tool to validate that your GASD files are semantically and syntactically correct.
- [**VS Code Extension**](https://github.com/General-Agentic-Software-Design/gasd-vscode-extension): Official VS Code support for GASD, featuring syntax highlighting for a better design experience.

## 📂 Repository Structure

- [**GASD/**](./GASD): Contains the official GASD 1.1 Specification and the self-defining metamodel.
- [**Build/**](./Build): Contains robust templates for task lifecycles, including:
    - `build_plan.gasd`: A comprehensive template covering Design, Validation, Testing, Execution, Quality, and Delivery.

## 🛠️ Key GASD Primitives

- `TYPE`: Define strict data contracts and structures with validation annotations.
- `FLOW`: Structured workflow steps with pre-conditions and post-conditions.
- `COMPONENT`: Functional building blocks and specialized agent roles.
- `DECISION`: Locked-down architectural or process choices with rationales.
- `CONSTRAINT & INVARIANT`: Global rules the agent MUST obey throughout the task.

## 📖 Quick Example: A Task Workflow

Instead of vague markdown instructions, a GASD `FLOW` provides a clear, numbered sequence of goals with explicit failure handlers:

```gasd
FLOW generate_report(data: InputData):
    1. ACHIEVE "Review all sources for requested scope"
    2. ENSURE "No outdated information is included"
        OTHERWISE THROW "Validation Error: Outdated Data Detected"
    3. ACHIEVE "Generate comprehensive analysis" @trace #REQ-001
    4. ENSURE "All key metrics are covered in report"
        OTHERWISE THROW "Coverage Gap"
```

## ⚖️ License

Distributed under the MIT License. See `LICENSE` for more information.
