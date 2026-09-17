## Context

`llm/vertex.go` defines the Vertex synthesis retry count, base delay, and maximum delay in a package-level constant block. The same provider currently embeds `300 * time.Second` in `NewVertexSynthesizer` and `16000` in `Synthesize`, even though comments identify both as intentional policy values. This design extracts those literals without changing their values or the behavior covered by `llm/vertex_test.go`.

The proposal records PASS assessments for Autonomous Collaboration, Composability First, Observable Quality, and Testability. This implementation preserves those assessments because it changes only the internal representation of fixed provider policy.

## Goals / Non-Goals

### Goals
- Give the Vertex synthesis timeout and output-token limit descriptive package-private names.
- Keep the timeout at 300 seconds and `max_tokens` at 16,000.
- Keep the change within the existing constant block and the two current call sites.
- Preserve all public interfaces and observable synthesis behavior.

### Non-Goals
- Making the timeout or output-token limit configurable.
- Changing Vertex embedding policy or the legacy Ollama synthesis timeout.
- Changing request, retry, authentication, endpoint, or response behavior.
- Adding tests that couple to package-private constant names.
- Updating user documentation for a non-user-facing refactor.

## Decisions

### D1: Add constants to the existing Vertex synthesis policy block

Add `vertexSynthTimeout` and `vertexSynthMaxTokens` beside `vertexSynthMaxRetries`, `vertexSynthBaseDelay`, and `vertexSynthMaxDelay`. This keeps related fixed policy values together and follows the existing package naming pattern and CS-007 guidance on magic numbers.

This decision preserves Composability First and Autonomous Collaboration: it introduces no dependencies, package coupling, MCP changes, or runtime communication changes.

### D2: Move rationale comments to the constant declarations

Place concise comments above the constants explaining why large curation prompts require the current timeout and token limit. The two call sites then refer only to the named constants, preventing duplicated or detached rationale.

This improves source auditability while preserving Observable Quality. Runtime results and provenance metadata remain unchanged.

### D3: Preserve values exactly and avoid new tests

Replace only `300 * time.Second` and `16000` with their equivalent constants. Existing tests already cover constructor and request behavior without external services. A test that asserts unexported identifier names would verify implementation structure rather than observable behavior, so the implementation will rely on existing tests plus formatting, build, vet, race-enabled tests, and quality gates.

## Coverage Strategy

- **Existing contract coverage**: Run the current `llm` and repository test suites to verify synthesis requests, responses, errors, retries, and cancellation remain unchanged.
- **Structural verification**: Review the diff to confirm both call sites use the named constants and their values remain identical.
- **CI parity**: Read `.github/workflows/` during implementation and execute the exact build, vet, race-enabled test, lint, and Gaze commands required there.

## Risks / Trade-offs

- **Risk**: A value could be changed accidentally during extraction. **Mitigation**: Specify the exact existing values and verify the final diff before running CI-equivalent checks.
- **Trade-off**: The constants remain compile-time policy rather than configuration. This is intentional because configurability would expand scope and alter user-facing behavior.
- **Trade-off**: No new unit test enforces the private names. This avoids brittle implementation-detail coverage while existing behavioral tests continue to protect the provider contract.
<!-- scaffolded by uf v0.17.0 -->
