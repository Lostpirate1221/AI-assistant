# Zero Delta Security AI Assistant

A local-first AI cybersecurity assistant being developed by **Zero Delta Security (ZDS)** for authorized security research, website intelligence, defensive analysis, and assessment support.

The project uses a locally hosted language model through Ollama and a Python-based agent framework. The goal is to keep the core workflow under the operator's control while building practical tools for repeatable security assessments.

## Current Capabilities

- **Local model integration:** Runs with a locally hosted model through Ollama.
- **Website retrieval:** Can retrieve public web content through the agent's fetch workflow.
- **Deterministic tool handling:** Includes an allowlisted fallback for interpreting supported tool-call requests.
- **Basic utility tools:** Includes calculation and runtime information tools.
- **Untrusted-content handling:** Retrieved website content is treated as external data, not as instructions to the agent.

> Project status: Active development. Website retrieval has been demonstrated. Structured page parsing, technology fingerprinting, security-header analysis, and automated assessment reporting are planned improvements and should not be considered implemented until documented and tested.

## Project Goals

1. Build repeatable passive website intelligence collection.
2. Identify observable technologies and security-relevant HTTP configuration.
3. Support authorized web and API security assessments.
4. Add controlled, scope-aware validation workflows.
5. Produce clear findings with evidence, confidence, severity, and remediation guidance.
6. Keep useful assessment history and generate professional reports.

## Planned Development

- HTTP response and page metadata extraction
- Security-header inspection
- Internal and external link discovery
- Form and input-field inventory
- Technology fingerprinting from observable evidence
- Scope controls and safe assessment profiles
- Structured JSON results and human-readable reports
- Automated tests for tool routing and analysis workflows

## Safety and Intended Use

This project is intended for systems the operator owns or has explicit permission to assess.

- Respect written authorization and defined testing scope.
- Do not treat retrieved web content as trusted agent instructions.
- Do not perform exploitation, authentication attempts, or intrusive testing unless specifically authorized and implemented in a controlled workflow.
- Protect credentials, tokens, client data, and assessment artifacts.
- Validate findings before reporting them.

## Development Environment

- Python
- Ollama
- Locally hosted language model (currently configured with Mistral)
- Linux/Kali development environment

## Project Status

This repository is under active development. Features and documentation will be updated as capabilities are implemented and tested.

## About Zero Delta Security

**Zero Delta Security (ZDS)** focuses on ethical security testing, attack-surface discovery, defensive hardening, automation, and practical security education.

---

*Built for responsible security research. Verify, document, and improve.*
