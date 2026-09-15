<!--
  [P] marks tasks eligible for parallel execution.
  Add [P] when a task: (a) touches different files from
  other [P] tasks in the group, (b) has no dependency
  on prior tasks in the group, (c) can safely execute
  without ordering constraints.
  Do NOT add [P] when tasks modify the same file —
  parallel workers will cause merge conflicts.
  Tasks without [P] run sequentially first, then [P]
  tasks run in parallel.
-->

## Unleash Execution Checklist

- [x] Step 0: Startup Cleanup
- [x] Step 1: Branch Safety Gate
- [x] Step 2: Resumability Detection
- [x] Step 3: Clarify (Step 1)
- [x] Step 4: Plan (Step 2)
- [x] Step 5: Tasks (Step 3)
- [x] Step 6: Spec Review (Step 4) -- iteration: 2/3
- [ ] Step 7: Implement (Step 5) -- phase: 0/3, batch: 0/0, workers: 0/0
- [ ] Step 8: Code Review (Step 6) -- iteration: 0/3
- [ ] Step 9: Retrospective (Step 7)
- [ ] Step 10: Demo (Step 8)

## 1. Extract Vertex Synthesis Policy Constants

- [ ] 1.1 Add documented `vertexSynthTimeout` and `vertexSynthMaxTokens` constants to the existing constant block in `llm/vertex.go`, preserving values of `300 * time.Second` and `16000`.
- [ ] 1.2 Replace the matching inline literals in `NewVertexSynthesizer` and `Synthesize` with the new constants, leaving public interfaces and all other provider behavior unchanged.
- [ ] 1.3 Run `gofmt` on `llm/vertex.go` and inspect the diff to confirm the change is limited to the constant declarations, rationale comments, and two substitutions.

## 2. Verify Behavior And Quality Gates

- [ ] 2.1 Run `go mod download`, `go build -v ./...`, and `go vet ./...` as required by `.github/workflows/ci.yml`.
- [ ] 2.2 Run `go test -race -v -count=1 -coverprofile=coverage.out ./...` and confirm the existing isolated Vertex synthesis tests pass without external services.
- [ ] 2.3 Run the exact CI gate `gaze crap --format=json --coverprofile=coverage.out --max-crapload=48 --max-gaze-crapload=45 ./...`, then enforce the stricter documented gate with `gaze report ./... --coverprofile=coverage.out --max-crapload=48 --max-gaze-crapload=18 --min-contract-coverage=70`; both MUST pass.
- [ ] 2.4 Run the repository's MegaLinter-equivalent pre-flight check derived from `.github/workflows/mega-linter.yml` and resolve any findings caused by this change.

## 3. Governance And Documentation

- [ ] 3.1 Verify the completed diff still satisfies the proposal's PASS assessments for Autonomous Collaboration, Composability First, Observable Quality, and Testability.
- [ ] 3.2 Assess documentation impact and confirm no `README.md`, `AGENTS.md`, GoDoc, or website documentation update is required because the refactor changes no exported or user-facing behavior.
- [ ] 3.3 Mark each completed task checkbox immediately and retain the exact timeout and token values required by the delta specification.
<!-- scaffolded by uf v0.17.0 -->
<!-- spec-review: passed -->
