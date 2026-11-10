# Development Roadmap

## Purpose

This roadmap defines the planned evolution of the Zero Delta Security AI Assistant from a local model-and-tools prototype into a dependable assistant for authorized security assessment workflows.

The roadmap is milestone-based. A phase is complete only when its implementation, tests, documentation, and safety requirements are met. A feature should not be described as operational merely because it appears in a plan or prompt.

## Development Principles

- **Local-first:** Keep model inference and core orchestration under operator control where practical.
- **Least privilege:** Give tools narrow, explicit capabilities.
- **Authorization before action:** Enforce assessment scope in application code at execution time.
- **Evidence-driven findings:** Separate observed facts from model interpretation and hypotheses.
- **Safe failure:** Reject malformed, unsupported, or out-of-scope requests.
- **Testable changes:** Add automated tests with each meaningful capability.
- **Professional output:** Make results reproducible, traceable, and understandable to technical and non-technical readers.

## Phase 1: Stabilize the Agent Foundation

**Goal:** Make the existing local agent predictable, testable, and maintainable.

### Work items

- [x] Establish Python agent orchestration.
- [x] Connect the agent to Ollama with the current Mistral configuration.
- [x] Implement registered tool dispatch.
- [x] Add deterministic fallback handling for supported tool-call output.
- [x] Add unit tests for fallback behavior.
- [ ] Document the complete tool interface and argument schemas.
- [ ] Standardize tool result and error formats.
- [ ] Add consistent logging with sensitive-data redaction.
- [ ] Add configuration validation and clear startup diagnostics.
- [ ] Add timeout and failure handling for external requests.

### Completion criteria

- The agent starts reliably from a documented setup.
- Supported tool calls execute only through registered handlers.
- Unknown tools and invalid arguments are rejected safely.
- Unit tests cover successful calls, malformed calls, unknown tools, and error paths.
- Logs do not expose credentials or other sensitive values.

## Phase 2: Passive Website Intelligence

**Goal:** Turn basic URL retrieval into a structured, repeatable website-intelligence workflow.

### Work items

- [ ] Normalize HTTP response status, final URL, and response metadata.
- [ ] Extract the HTML page title and relevant metadata.
- [ ] Inventory internal and external links.
- [ ] Identify forms and input fields from retrieved HTML.
- [ ] Inspect security-related HTTP response headers.
- [ ] Record observable technology indicators with supporting evidence.
- [ ] Handle redirects, unsupported content types, oversized responses, and timeouts.
- [ ] Produce a consistent JSON result schema.
- [ ] Add fixture-based tests using local sample pages.

### Completion criteria

- A permitted public URL can be assessed without attempting exploitation.
- Output distinguishes confirmed observations from inferred technology guesses.
- Parsing failures are reported without crashing the agent.
- Results are reproducible and include the source URL and collection time.
- Tests cover normal pages, malformed HTML, missing headers, redirects, and error responses.

## Phase 3: Scope and Authorization Enforcement

**Goal:** Make authorization a technical control rather than relying on operator prompts or model behavior.

### Work items

- [ ] Define an explicit assessment-scope format.
- [ ] Support exact hostnames and clearly defined allowed URL patterns.
- [ ] Reject out-of-scope destinations before network access.
- [ ] Validate redirects against the same scope policy.
- [ ] Define permitted assessment profiles, beginning with passive collection.
- [ ] Require explicit operator approval for higher-impact operations.
- [ ] Record scope decisions and action outcomes in an audit trail.
- [ ] Add tests for allowed, denied, ambiguous, and redirected destinations.

### Completion criteria

- Every assessment-related network action passes through a centralized scope check.
- Redirects cannot silently move an operation outside the approved scope.
- Denied actions fail closed and produce an understandable audit event.
- Scope enforcement is tested independently of model-generated instructions.

## Phase 4: Security Analysis and Finding Validation

**Goal:** Convert collected evidence into careful, defensible security observations.

### Work items

- [ ] Add passive checks for missing or weak security headers.
- [ ] Identify observable TLS and transport configuration details where supported.
- [ ] Define a finding schema with title, description, evidence, severity, confidence, and remediation.
- [ ] Distinguish informational observations from validated vulnerabilities.
- [ ] Add duplicate detection and evidence references.
- [ ] Require human review before findings are treated as confirmed.
- [ ] Add controlled validation checks only for explicitly authorized targets.

### Completion criteria

- Every finding links to concrete evidence.
- Severity and confidence are separate fields.
- The system does not label an unverified indicator as a confirmed vulnerability.
- Active checks are unavailable unless the target, method, and authorization requirements are satisfied.
- Tests verify both positive and negative cases.

## Phase 5: Reporting and Assessment History

**Goal:** Produce professional outputs suitable for internal review and client-facing assessment workflows.

### Work items

- [ ] Export structured JSON assessment results.
- [ ] Generate a readable Markdown report.
- [ ] Add an executive summary and technical findings sections.
- [ ] Include scope, collection time, methodology, limitations, and remediation guidance.
- [ ] Track assessment identifiers and report versions.
- [ ] Store assessment artifacts in a controlled local location.
- [ ] Define retention and redaction rules for sensitive data.

### Completion criteria

- Reports can be regenerated from structured assessment data.
- Findings retain evidence and traceability.
- Sensitive information is redacted according to documented rules.
- Report limitations and unverified observations are clearly identified.

## Phase 6: Operational Hardening and Release

**Goal:** Prepare the assistant for repeatable use in ZDS lab and authorized client workflows.

### Work items

- [ ] Add dependency pinning and a documented installation process.
- [ ] Add configuration examples without real secrets.
- [ ] Add a comprehensive test command and CI workflow.
- [ ] Add security review for tool permissions and data handling.
- [ ] Document backup, update, and recovery procedures.
- [ ] Add versioning and release notes.
- [ ] Run an end-to-end assessment against a deliberately controlled test environment.

### Completion criteria

- A clean environment can be installed using repository instructions.
- Automated tests run consistently.
- No secrets or private assessment data are committed.
- The end-to-end workflow respects scope and produces reproducible output.
- Release documentation accurately describes supported and unsupported features.

## Immediate Next Milestones

The next practical sequence is:

1. Standardize the tool result format and error handling.
2. Inspect and document the current `fetch_url` implementation.
3. Implement passive HTML and response-metadata extraction.
4. Add security-header inspection.
5. Add fixture-based tests for the website-intelligence layer.
6. Connect scope enforcement before expanding assessment capabilities.

## Status Tracking

Update this document as work is completed. Use checked boxes only for functionality that has been implemented and verified. For larger milestones, link to the relevant code, tests, or design document.

---

**Project:** Zero Delta Security AI Assistant  
**Organization:** Zero Delta Security (ZDS)  
**Roadmap status:** Active development plan
