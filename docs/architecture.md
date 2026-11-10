# System Architecture

## 1. Overview

Zero Delta Security AI Assistant is a local-first, tool-using cybersecurity assistant developed by Zero Delta Security (ZDS). Its current implementation combines a Python orchestration layer with a locally hosted language model served by Ollama.

The architecture is designed to keep model reasoning separate from actions that interact with external systems. The model can request supported tools, while Python-side logic determines whether a requested operation matches an explicitly supported tool and its input contract.

The current project is an early-stage foundation. Public website retrieval and the local model workflow have been demonstrated. A complete reconnaissance pipeline, automated vulnerability testing framework, and production reporting system are not yet represented as completed capabilities.

## 2. High-Level Architecture

```text
                  Operator
                     |
                     v
             Python Agent Interface
                     |
                     v
          Agent Orchestration (agent.py)
             |                 |
             |                 v
             |          Tool-Call Handling
             |          - Parse model output
             |          - Validate tool name
             |          - Enforce allowlist
             |          - Validate arguments
             |                 |
             v                 v
       Ollama Runtime <----> Approved Tool Functions
             |                 |
             v                 +--> Public URL retrieval
       Mistral Model           +--> Calculation
                               +--> Runtime information
                               +--> Other registered tools
                                      |
                                      v
                               Tool result returned
                                      |
                                      v
                              Agent / Model response
                                      |
                                      v
                                   Operator
```

This diagram represents the logical flow, not a claim that every planned security-analysis component is already connected to the execution path.

## 3. Core Components

### 3.1 Python agent and orchestration

The Python agent is the application control layer. It manages the interaction loop, prepares model messages, processes model responses, dispatches supported tools, and returns results to the operator.

The primary orchestration implementation is maintained in `agent.py` in the local development project.

Responsibilities include:

- Maintaining the conversation and model interaction flow.
- Providing tool definitions and supported operations to the model.
- Interpreting tool-call requests.
- Routing approved requests to Python functions.
- Returning tool output to the conversation.
- Applying deterministic handling where model output does not arrive in the expected structured form.

### 3.2 Local inference through Ollama

The agent uses Ollama to run a language model locally. The current configured model is `mistral:latest`.

Ollama provides the local inference runtime. The Python application sends conversation context and tool definitions to the model, then receives a response that may contain a normal answer or a request to use a supported tool.

The model proposes actions; it does not independently receive unrestricted operating-system or network access. Tool execution is handled by the Python application.

### 3.3 Tool registry and controlled dispatch

Tools expose specific functions to the agent. Examples in the current implementation include:

| Tool | Purpose | Current boundary |
|---|---|---|
| `fetch_url` | Retrieve content from a public HTTP or HTTPS URL | Retrieval only; not browser automation |
| `calculate` | Evaluate supported basic arithmetic | Limited utility operation |
| `get_runtime_info` | Return agent runtime details | Informational operation |

Tool availability is controlled by the application's registered tool definitions and allowlisted dispatch logic. A model-generated tool name should not be treated as executable merely because it appears in a response.

The deterministic fallback path is intended to recognize supported tool-call requests when the model emits a compatible textual representation instead of the expected structured call format. The fallback must remain constrained by the same allowlist and argument validation as the normal dispatch path.

### 3.4 Website retrieval

The current `fetch_url` workflow retrieves public content over HTTP or HTTPS and makes the retrieved result available to the agent for analysis.

Retrieval is not equivalent to full browser interaction. It does not, by itself, establish support for JavaScript execution, authenticated browsing, interactive page workflows, crawling, or vulnerability exploitation.

Content returned from an external website is untrusted input. It may contain misleading text or instructions designed to influence the model. The agent must treat such content as data to analyze, not as higher-priority instructions or authorization to perform actions.

### 3.5 Scope and authorization controls

The broader local project includes scope-management and authorization-related components, along with audit-oriented controls and read-only filesystem tooling.

These components are intended to support responsible assessment workflows. Their presence should not be interpreted as proof that every security tool is already gated by them. At the current documented checkpoint, the authorization helper had not yet been connected to security-tool execution.

A key architectural requirement for future development is to make scope and authorization checks mandatory at the execution boundary, rather than relying on model instructions alone.

### 3.6 Tests

The project includes Python unit tests for tool-call fallback behavior. The test suite checks supported tool-call extraction and related routing behavior without requiring live target interaction.

Tests should be expanded alongside new features, particularly around:

- Tool allowlist enforcement.
- Invalid or missing tool arguments.
- URL scheme and destination validation.
- Untrusted retrieved content.
- Scope and authorization enforcement.
- Error handling and timeouts.
- Stable, structured assessment output.

## 4. Request Lifecycle

A typical interaction follows this sequence:

1. **Operator input:** The operator submits a question or requests a supported operation.
2. **Agent preparation:** The Python application prepares the conversation context and available tool definitions.
3. **Model inference:** Ollama runs the configured Mistral model and returns a response.
4. **Response classification:** The agent determines whether the response is a normal answer or a tool request.
5. **Tool validation:** The application checks that the requested tool is registered and that its arguments satisfy the expected contract.
6. **Controlled execution:** The corresponding Python function runs within its defined behavior.
7. **Result handling:** The tool result is returned to the agent and incorporated into the response.
8. **Operator output:** The agent presents the result, ideally distinguishing observed evidence from interpretation.

If a tool request is malformed, unknown, or outside the supported contract, it should be rejected rather than guessed or executed.

## 5. Security Design Principles

The architecture is being developed around the following principles:

### Least privilege

Expose narrowly defined tools instead of unrestricted shell, filesystem, or network access. Add capabilities only when there is a clear operational need and a defined security boundary.

### Explicit authorization and scope

Security assessment actions must be limited to assets for which the operator has permission. Scope validation should occur in application code immediately before an action is executed.

### Untrusted external content

Web pages, documents, and tool output are data. They must not be allowed to override system policy, grant authorization, or silently trigger unrelated actions.

### Deterministic tool boundaries

Tool names, arguments, URL schemes, and operation types should be validated in code. Model-generated text is not a security control.

### Auditability

Assessment activity should be traceable through structured logs that record relevant actions and outcomes without unnecessarily storing credentials, secrets, or sensitive client data.

### Human oversight

High-impact, intrusive, destructive, authenticated, or externally visible actions should require explicit authorization and appropriate operator review.

## 6. Current Implementation vs. Planned Work

| Area | Status |
|---|---|
| Python agent orchestration | Implemented |
| Ollama local inference | Implemented |
| Mistral model configuration | Implemented |
| Registered tool dispatch | Implemented |
| Deterministic tool-call fallback | Implemented and covered by unit tests |
| Public HTTP/HTTPS retrieval | Demonstrated |
| Basic calculation and runtime tools | Implemented |
| Scope and authorization helper components | Present in the local project; integration with security-tool execution remains incomplete |
| Structured page metadata and link extraction | Planned |
| Security-header assessment | Planned |
| Technology fingerprinting | Planned |
| Automated vulnerability validation | Planned |
| Client-ready assessment reporting | Planned |

Statuses describe the documented development checkpoint and should be updated as implementation changes.

## 7. Planned Architectural Evolution

The next stage is to build a passive website-intelligence layer between retrieval and reporting:

1. Normalize HTTP response metadata.
2. Extract page title, metadata, links, forms, and input fields from retrieved content.
3. Record observable technology indicators with evidence and confidence.
4. Inspect security-relevant response headers.
5. Produce structured findings in a stable machine-readable format.
6. Add scope enforcement and audit logging to every applicable assessment action.
7. Add tests for parsing, validation, error handling, and safe failure behavior.

Only after these foundations are reliable should the project expand into controlled active testing workflows.

## 8. Repository and Development Notes

The GitHub repository is the public project record. The local development environment currently uses Python, a project virtual environment, Ollama, and the Mistral model configuration.

Do not commit:

- API keys, passwords, access tokens, or private keys.
- Client credentials, private assessment data, or sensitive logs.
- Local environment files such as `.env`.
- Unredacted target data without explicit permission.

Keep implementation status, test results, and architecture documentation synchronized as the project evolves.

---

**Project:** Zero Delta Security AI Assistant  
**Organization:** Zero Delta Security (ZDS)  
**Architecture status:** Early-stage, actively evolving  
