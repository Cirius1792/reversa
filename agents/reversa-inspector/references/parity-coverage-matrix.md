# Parity Coverage Matrix

Reference table for defining the minimum set of `.feature` scenarios per flow, according to paradigm transition.

## Coverage by Transition

| Transition | Minimum Scenarios per Flow |
|---|---|
| no change | `@paridade` (input → expected output) |
| procedural → OO | `@paridade` + `@invariante` (validated aggregate invariant) |
| procedural → event-driven | `@paridade` + `@idempotencia` + `@ordem` + `@dlq` (behavior under queue failure) |
| classic OO → OO with DI | `@paridade` + `@composicao` (no Active Record dependency) |
| classic OO → event-driven | `@paridade` + `@idempotencia` + `@ordem` + `@saga` (compensation on failure) |
| classic OO → functional | `@paridade` + `@imutabilidade` + `@composicao` |
| OO with DI → event-driven | `@paridade` + `@idempotencia` + `@ordem` |
| functional → event-driven | `@paridade` + `@idempotencia` + `@ordem` |
| any → actor model | `@paridade` + `@supervisao` (recovery after failure) |

## Conventional Tags

- `@paridade`: always present; primary equivalence.
- `@critico`: critical flow (regulatory, financial, sensitive data).
- `@regulatorio`: when there is an external formal requirement.
- `@idempotencia`: reprocessing does not duplicate effect.
- `@ordem`: order by key respected.
- `@dlq`: behavior when reaching dead letter queue.
- `@saga`: compensation in distributed transaction.
- `@invariante`: validated aggregate invariant.
- `@composicao`: equivalent behavior under functional composition.
- `@imutabilidade`: no shared mutation.
- `@supervisao`: supervisor recovers failed actor.

## Typical "Accepted Parity" Criteria

| System Type | Primary Metric |
|---|---|
| Web app without strong regulation | functional divergence < 1% over 7 days |
| Public API | functional divergence < 0.1% over 30 days + zero divergence in public contracts |
| Fiscal / regulatory system | functional divergence < 0.01% over 60 days + zero divergence in regulated fields |
| Financial system | financial divergence by monetary value < 0.001% + zero divergence in totals |
| Low-criticality internal system | functional divergence < 5% over 7 days |

## Reuse of characterization_specs

When `_reversa_sdd/characterization_specs/` exists:

1. For each spec → derive the corresponding `.feature`, adapting inputs/outputs to the new system.
2. Keep the original `spec-id` in traceability.
3. Add extra scenarios according to the "Minimum Scenarios per Flow" table.

When it does not exist:

1. Infer critical flows from `code-analysis.md` + `sequences/` + `BR-MIGRAR` rules marked as critical.
2. Document the gap in `parity_specs.md § Reuse of characterization_specs`.
