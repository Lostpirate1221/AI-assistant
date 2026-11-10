# Development Roadmap

## Mission

Develop a local-first, operator-controlled AI assistant that supports authorized cybersecurity work for Zero Delta Security (ZDS). The project will progress from a dependable agent foundation to evidence-driven website intelligence, scope-aware assessment workflows, and professional reporting.

This roadmap is an engineering plan, not a claim that every listed capability currently exists. A milestone is complete only when its implementation is tested, documented, and reviewed against its acceptance criteria.

## Engineering Principles

1. **Local control:** Prefer local inference and transparent, operator-controlled execution.
2. **Least privilege:** Expose narrow tools with explicit input and output contracts.
3. **Authorization at execution time:** Enforce target scope in application code, not solely through model instructions.
4. **Evidence before conclusions:** Distinguish observations, inferences, and confirmed vulnerabilities.
5. **Safe failure:** Reject unsupported tools, malformed arguments, and out-of-scope actions.
6. **Reproducibility:** Produce structured outputs and retain enough context to explain how a result was obtained.
7. **Privacy by design:** Avoid storing secrets and minimize retention of sensitive client data.

## Phase 1 - Agent Foundation

**Objective:** Make the local agent predictable, testable, and maintainable.

### Work items

- [x] Python-based agent orchestration.
- [x] Local inference through Ollama.
- [x] Mistral model configuration.
- [x] Registered tool dispatch.
- [x] Deterministic fallback for supported tool-call output.
- [x] Unit tests for tool-call fallback behavior.
- [ ] Document every registered tool and its argument contract.
- [ ] Standardize tool success and error response schemas.
- [ ] Add consistent logging with secret redaction.
- [ ] Validate configuration at startup.
- [ ] Add explicit network timeouts and response-size limits.

### Acceptance criteria

- A clean environment can start the agent using documented instructions.
- Only registered tools can execute.
- Invalid tool names and arguments fail closed.
- Automated tests cover successful, malformed, unknown, and failing tool calls.
- Logs do not expose credentials, tokens, or sensitive client data.

## Phase 2 - Passive Website Intelligence

**Objective:** Evolve basic URL retrieval into a structured, repeatable website-intelligence workflow.

### Work items

- [ ] Normalize HTTP status, requested URL, final URL, and redirect information.
- [ ] Capture response headers and content type.
- [ ] Extract page title and relevant metadata.
- [ ] Inventory internal and external links.
- [ ] Identify forms and input fields from retrieved HTML.
- [ ] Inspect security-related response headers.
- [ ] Record observable technology indicators with supporting evidence.
- [ ] Handle redirects, unsupported content types, oversized responses, and timeouts.
- [ ] Define a stable JSON output schema.
- [ ] Add fixture-based tests using local sample pages.

### Acceptance criteria

- A permitted URL can be assessed using passive retrieval only.
- Results include the source URL and collection timestamp.
- Confirmed observations are clearly separated from technology guesses.
- Parsing failures are reported without crashing the agent.
- Tests cover ordinary pages, malformed HTML, missing headers, redirects, and HTTP errors.

## Phase 3 - Scope and Authorization Enforcement

**Objective:** Make authorization a technical execution control rather than relying on prompts or operator memory.

### Work items

- [ ] Define a machine-readable assessment-scope format.
- [ ] Support explicit hostnames and carefully defined URL patterns.
- [ ] Validate the destination before every assessment-related network request.
- [ ] Revalidate every redirect against the approved scope.
- [ ] Define assessment profiles, beginning with passive collection.
- [ ] Require explicit operator approval for higher-impact operations.
- [ ] Record allow/deny decisions and action outcomes in an audit trail.
- [ ] Test allowed, denied, ambiguous, and redirected destinations.

### Acceptance criteria

- Every assessment-related network action passes through centralized scope validation.
- Redirects cannot silently move a request outside the approved scope.
- Denied operations fail closed with a clear reason.
- Scope enforcement is tested independently of model-generated instructions.

## Phase 4 - Security Analysis and Validation

**Objective:** Convert collected evidence into careful, defensible security observations.

### Work items

- [ ] Add passive checks for missing or weak security headers.
- [ ] Capture supported TLS and transport configuration observations.
- [ ] Define a finding schema: title, description, evidence, severity, confidence, and remediation.
- [ ] Separate informational observations from validated vulnerabilities.
- [ ] Add duplicate detection and evidence references.
- [ ] Require human review before a finding is marked confirmed.
- [ ] Introduce controlled validation checks only for explicitly authorized targets.

### Acceptance criteria

- Every finding is traceable to concrete evidence.
- Severity and confidence are represented separately.
- Unverified indicators are not presented as confirmed vulnerabilities.
- Active checks require explicit scope and authorization controls.
- Tests cover positive, negative, and inconclusive outcomes.

## Phase 5 - Reporting and Assessment History

**Objective:** Produce consistent outputs suitable for internal review and authorized client engagements.

### Work items

- [ ] Export structured JSON assessment results.
- [ ] Generate readable Markdown reports.
- [ ] Add executive summaries and technical findings.
- [ ] Include scope, collection time, methodology, limitations, and remediation guidance.
- [ ] Track assessment identifiers and report versions.
- [ ] Store artifacts in a controlled local location.
- [ ] Define retention, redaction, and deletion procedures for sensitive data.

### Acceptance criteria

- Reports can be regenerated from structured assessment data.
- Findings preserve their evidence and traceability.
- Sensitive information is handled according to documented rules.
- Report limitations and unverified observations are explicit.

## Phase 6 - Operational Hardening and Release

**Objective:** Prepare the assistant for repeatable use in ZDS lab and authorized client workflows.

### Work items

- [ ] Pin and document Python dependencies.
- [ ] Provide installation and configuration instructions.
- [ ] Add safe example configuration without real secrets.
- [ ] Add a repeatable test command and continuous integration workflow.
- [ ] Review tool permissions, network behavior, and data handling.
- [ ] Document backup, update, and recovery procedures.
- [ ] Introduce versioning and release notes.
- [ ] Run an end-to-end assessment against a deliberately controlled test environment.

### Acceptance criteria

- A clean environment can be installed from repository instructions.
- Automated tests run consistently.
- No secrets or private assessment data are committed.
- The end-to-end workflow respects scope and produces reproducible results.
- Release notes accurately describe supported and unsupported capabilities.

## Immediate Engineering Priorities

1. Inspect and document the current `fetch_url` implementation and its limits.
2. Standardize tool result and error formats.
3. Implement passive response and HTML metadata extraction.
4. Add security-header inspection.
5. Build fixture-based tests for website parsing.
6. Connect centralized scope enforcement before adding active assessment features.

## Status and Change Control

- Check off work only after implementation and verification.
- Link significant milestones to relevant code, tests, or design documents.
- Update acceptance criteria when scope changes.
- Keep the README, architecture document, and this roadmap synchronized.
- Record material design decisions so future contributors can understand why a control exists.

---

**Project:** Zero Delta Security AI Assistant  
**Organization:** Zero Delta Security (ZDS)  
**Status:** Active development
