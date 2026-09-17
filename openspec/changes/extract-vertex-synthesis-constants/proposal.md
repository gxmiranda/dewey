## Why

The Vertex AI synthesizer keeps its retry policy in named constants, but its HTTP timeout and maximum output token limit remain inline literals. Extracting those values makes the provider policy easier to identify and maintain, and aligns `llm/vertex.go` with the project's convention to avoid magic numbers without changing runtime behavior.

## What Changes

- Add package-private named constants for the existing 300-second Vertex synthesis timeout and 16,000-token output limit.
- Replace the matching inline literals in the HTTP client and request payload with those constants.
- Preserve the current values, request format, retry behavior, and public interfaces.
- Verify the refactor with formatting and the repository's CI-equivalent checks; do not add tests that assert private implementation details.

## Capabilities

### New Capabilities
- `vertex-synthesis-policy`: Defines how fixed Vertex AI synthesis policy values are represented while preserving the provider's externally observable behavior.

### Modified Capabilities
- None.

### Removed Capabilities
- None.

## Impact

The implementation is limited to `llm/vertex.go`. It adds no dependencies, configuration fields, exported APIs, persistence changes, or user-facing behavior. Existing `llm/vertex_test.go` coverage remains applicable because the refactor does not change the HTTP timeout value or serialized `max_tokens` value.

## Constitution Alignment

Assessed against the Unbound Force org constitution.

### I. Autonomous Collaboration

**Assessment**: PASS

The change does not alter MCP tools, structured responses, or communication between Dewey and agent personas. Existing artifact-based collaboration remains unchanged.

### II. Composability First

**Assessment**: PASS

The change adds no dependency or runtime coupling. Dewey and the Vertex synthesis provider remain independently usable under the existing opt-in configuration.

### III. Observable Quality

**Assessment**: PASS

The refactor preserves all observable output, provenance, health reporting, and provider behavior. Named constants make the fixed provider policy easier to audit in source without changing machine-readable results.

### IV. Testability

**Assessment**: PASS

The existing isolated `llm` tests continue to verify Vertex request and response behavior without external services. Because this change only names existing literals, implementation-detail assertions would not improve contract coverage; CI-equivalent formatting, build, vet, race-enabled tests, and quality checks will verify the change.
<!-- scaffolded by uf v0.17.0 -->
