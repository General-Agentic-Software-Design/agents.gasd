# GASD Development Workflow

This guide describes a development workflow using General Agentic Software & Design (GASD). It takes a project from user stories through design, validation, test generation, implementation, quality assurance, and packaging, with traceability across each stage.

`Build/build_plan.gasd` is the source of truth for workflow execution: it defines the scope, sequence, inputs, outputs, and completion criteria for each phase. The agent templates provide reusable instructions for carrying out that plan.

## 1. Create the Project Structure

Use the following layout to separate agent instructions, requirements, specifications, implementation, and release artifacts. The workflow generates the output files and directories as needed.

```text
my-project/
├── agent.gasd/
│   ├── build.gasd
│   ├── code-agent.gasd
│   ├── design-agent.gasd
│   ├── full_quality_gate.gasd
│   ├── packaging-agent.gasd
│   ├── test-gen-agent.gasd
│   ├── user-story-agent.gasd
│   └── validation-agent.gasd
├── Build/
│   ├── build_plan.gasd
│   └── Installation/
├── Requirements/
│   └── User-Stories.md
├── Design/
├── Validation/
│   ├── Design/
│   ├── Acceptance/
│   ├── Regression/
│   └── Negative/
├── Impl/
│   └── tests/
└── GASD-1.2/
    └── GASD_Specification.md
```

## 2. Prepare the Specification, Templates, and Tools

Copy the following resources from this repository into the project:

| Resource | Repository source | Project destination |
| --- | --- | --- |
| GASD 1.2 specification | [GASD_Specification.md](../GASD-1.2/GASD_Specification.md) | `GASD-1.2/GASD_Specification.md` |
| Agent templates | [agent.gasd-template/](../agent.gasd-template/) | `agent.gasd/` |
| Build-plan template | [build_plan.gasd](../Build/build_plan.gasd) | `Build/build_plan.gasd` |

Before an agent creates or edits GASD artifacts, instruct it to read the project’s copy of the GASD 1.2 specification and apply its syntax and constraints.

Install the [GASD Parser and Validator](https://github.com/General-Agentic-Software-Design/GASD-Parser-Validator) and record the project’s validation command in the build plan. Use it to validate every generated or manually edited GASD file. The build-plan template references `gasd_parser --ast-sem` for syntax and semantic checks; configure the complete invocation for the project’s files and installed tool version.

For editing in VS Code, optionally install the [GASD extension](https://github.com/General-Agentic-Software-Design/gasd-vscode-extension) for syntax highlighting.

## 3. Configure the Agent Instructions

Choose either of these approaches:

- **Use the agent templates.** Ask the AI agent to populate each template with project-specific values before using it. Templates reduce the need to write repeated prompts.
- **Work directly from the build plan.** Ask the AI agent to populate and execute `Build/build_plan.gasd`, supplying the equivalent phase instructions directly.

Both approaches must follow the same build plan. Populate each template using the project context, relevant requirements, and the corresponding build-plan phase once it has been defined. Replace placeholders for paths, story IDs, target language, test frameworks, validation commands, and pause conditions as applicable.

Review the copied build plan and templates together before execution. Update sample paths, identifiers, and specification references to match the project layout above. In particular, the build-plan template contains a GASD 1.1 specification path and a sample requirements path that must be replaced.

For this workflow, configure Phase 4 to use `Design/` as the sole source of implementation behavior. The copied build-plan template also permits `Validation/` as an implementation input; revise that instruction to match this workflow before execution.

Execute one phase at a time and pause for review unless a human explicitly authorizes multiple phases. Resolve conflicting instructions or missing inputs before continuing.

## 4. Execute the Workflow

Complete requirements and build-plan preparation first, then execute the plan’s six phases in order:

```text
User stories → Build plan → Design → Validation → Tests → Implementation → Quality gate → Packaging
```

### Step 1: Define User Stories

Use the configured [user-story-agent.gasd](../agent.gasd-template/user-story-agent.gasd) instructions to generate `Requirements/User-Stories.md` from the product brief and reference materials.

Include epics, user stories, subtasks, detailed acceptance criteria, and traceability identifiers. Acceptance criteria must describe observable outcomes and cover relevant edge cases, negative cases, and error conditions.

**Completion criteria:** Requirements have consistent identifiers and enough detail to derive design specifications and tests without consulting implementation code.

### Step 2: Prepare the Build Plan

Use the configured [build.gasd](../agent.gasd-template/build.gasd) instructions, or instruct the agent directly, to populate `Build/build_plan.gasd` using `Requirements/User-Stories.md` as context.

Define all six phases: design, validation, test generation, implementation, quality assurance, and packaging. Specify each phase’s inputs, outputs, scope, commands, traceability requirements, and completion criteria. Keep any separate agent instructions aligned with this plan.

**Completion criteria:** The build plan is project-specific, contains no unresolved placeholders, and passes GASD validation.

### Step 3: Generate Design Specifications — Phase 1

Use the configured [design-agent.gasd](../agent.gasd-template/design-agent.gasd) instructions to generate GASD design specifications in `Design/` from `Requirements/User-Stories.md`, following Phase 1 of the build plan.

Define component behavior, interfaces, data contracts, constraints, and error handling. Preserve requirement traceability and use concrete, measurable postconditions.

**Completion criteria:** The design covers the required scope, traces back to the acceptance criteria, and passes GASD validation.

### Step 4: Generate Validation Specifications — Phase 2

Use the configured [validation-agent.gasd](../agent.gasd-template/validation-agent.gasd) instructions to derive validation specifications from the requirements and GASD design files.

Write the specifications to `Validation/Design/`, `Validation/Acceptance/`, `Validation/Regression/`, and `Validation/Negative/`. Define expected outcomes independently of implementation code so they can guide test generation.

**Completion criteria:** Validation specifications cover the required design behavior and acceptance criteria, retain traceability, and pass GASD validation.

### Step 5: Generate Tests — Phase 3

Use the configured [test-gen-agent.gasd](../agent.gasd-template/test-gen-agent.gasd) instructions to generate executable tests in `Impl/tests/` from `Requirements/`, `Design/`, and `Validation/`.

Include acceptance, regression, negative, and design-validation tests with meaningful assertions. Derive expected results from the specifications without using production implementation code as a reference.

For an existing project, run the existing tests unmodified before generating new tests, as required by the build plan’s regression gate. Keep that baseline check separate from test generation. Under the test-generation template, newly generated tests are executed later during the quality gate unless explicitly requested earlier.

**Completion criteria:** Tests contain real test logic, cover the specified scenarios, and retain traceability to requirements and validation specifications.

### Step 6: Generate Implementation Code — Phase 4

Use the configured [code-agent.gasd](../agent.gasd-template/code-agent.gasd) instructions to generate production code in `Impl/`, using only the GASD design specifications as the source of implementation behavior.

Implementation deliberately follows design, validation, and test generation. If a design is incomplete or ambiguous, resolve the gap in the design and update affected validation specifications and tests before continuing.

**Completion criteria:** The implementation conforms to the design, covers the requested scope, and builds successfully where a build step applies. Test execution follows in Phase 5 unless explicitly authorized earlier.

### Step 7: Run the Full Quality Gate — Phase 5

Use the configured [full_quality_gate.gasd](../agent.gasd-template/full_quality_gate.gasd) instructions to execute the complete acceptance, regression, negative, and design-validation test suites.

Resolve implementation defects against the design specifications and rerun the required suites until all tests pass. Record any specification changes and update affected artifacts to preserve consistency and traceability.

**Completion criteria:** Every required test passes, and the results are recorded for review.

### Step 8: Package the Release — Phase 6

Use the configured [packaging-agent.gasd](../agent.gasd-template/packaging-agent.gasd) instructions to assemble the validated implementation and supporting release artifacts in `Build/Installation/`.

Check consistency across requirements, design, validation, and implementation. Document development installation, production installation, and test execution. Verify installation in a clean environment and run smoke tests against the packaged application.

**Completion criteria:** The release artifacts are complete, installation succeeds, and smoke tests confirm basic functionality.
