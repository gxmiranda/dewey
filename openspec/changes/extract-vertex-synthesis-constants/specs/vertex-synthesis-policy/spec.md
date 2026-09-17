## ADDED Requirements

### Requirement: Named Vertex synthesis policy constants

The Vertex AI synthesizer MUST represent its fixed HTTP timeout and maximum output token limit as descriptive, package-private constants in `llm/vertex.go`. The timeout constant MUST equal `300 * time.Second`, and the maximum output token constant MUST equal `16000`.

#### Scenario: Vertex HTTP client keeps the current timeout
- **GIVEN** the Vertex synthesizer's HTTP client is constructed
- **WHEN** the client timeout is assigned
- **THEN** the assignment MUST use the named Vertex synthesis timeout constant
- **AND** the resulting timeout MUST remain 300 seconds

#### Scenario: Vertex request keeps the current output limit
- **GIVEN** the Vertex synthesizer creates an Anthropic Messages request
- **WHEN** the request's `max_tokens` value is assigned
- **THEN** the assignment MUST use the named Vertex synthesis maximum-token constant
- **AND** the serialized value MUST remain `16000`

### Requirement: Behavior-preserving extraction

Extracting the Vertex synthesis policy constants MUST NOT change public interfaces, provider configuration, request structure, retry behavior, or response handling.

#### Scenario: Existing synthesis behavior remains unchanged
- **GIVEN** the existing isolated Vertex synthesizer test suite
- **WHEN** the named constants replace the equivalent inline literals
- **THEN** all existing tests MUST pass without requiring an external Vertex AI service

## MODIFIED Requirements

None.

## REMOVED Requirements

None.
<!-- scaffolded by uf v0.17.0 -->
