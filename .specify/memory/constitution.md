# Health Stream XAI Constitution

## Core Principles

### I. Spec-First Delivery
Every bounded feature MUST go through Spec Kit
(`specify` → `plan` → `tasks` → `implement` → `converge`) before large code
changes. Prefer small, reviewable PRs against `develop`; changes to `main` MUST
go through a pull request. Ad-hoc multi-file feature work without a feature
spec is not allowed when the change is more than a trivial fix or chore.

### II. Auditability Over Cleverness
Prefer designs that make model inputs, predictions, explanations, and data
lineage inspectable. When predicted risk is high, explainability (e.g. SHAP)
MUST be part of product behavior—not a side notebook or offline-only artifact.
Opaque shortcuts that hide why a score was produced are not acceptable for
high-risk paths.

### III. No Temporal Leakage
Training and evaluation MUST respect point-in-time availability of labs and
vitals. Features and labels MUST NOT use information that would not have been
physically available at the prediction timestamp. Suspected leakage MUST block
merge until resolved or explicitly documented as out of scope for that slice.

### IV. Drift Awareness
Serving and training MUST assume input distributions can change over time.
Monitoring and alerts for data/concept drift are first-class product concerns,
not optional polish after “accuracy ships.” New serving or training paths MUST
leave a clear place for baseline comparison and alerting as those components
land.

### V. Simplicity & Portfolio Honesty
Ship the smallest vertical slice that demonstrates the architecture
(ingest → infer → explain → monitor). README and docs MUST distinguish planned
vs implemented capabilities. Do not overbuild microservices, brokers, or
orchestration before a working end-to-end path exists. Do not invent production
SLAs, HIPAA certification claims, or infra choices that the repo has not
adopted.

## Additional Constraints

- **Runtime & tooling:** Python ≥ 3.11; `uv` manages dependencies. Use
  FastAPI/uvicorn when HTTP serving is introduced.
- **Aspirational stack:** Kafka/Redpanda, `shap`, Evidently, Docker Compose, and
  MLflow remain planned until present in code. PRs MUST NOT require unused tools
  as hard gates.
- **Data:** Synthetic or demo clinical data only (e.g. MIMIC-IV-style). Real PHI
  MUST NOT enter the repository, logs, fixtures, or shared artifacts.
- **Testing:** Unit/contract tests for pure logic as it appears; integration
  tests for streaming and serving boundaries when those components exist.
- **Git hygiene:** Conventional commits. Secrets, credentials, and `.venv` MUST
  stay out of version control.

## Development Workflow

- Use Spec Kit skills in agent chat for feature work; keep constitution rules in
  mind during plan and review.
- Prefer PR-based flow into `develop`; protect `main` via pull request.
- Reviews SHOULD check leakage, PHI, auditability, and planned-vs-implemented
  honesty when the change touches those areas.
- Complexity beyond the current vertical slice MUST be justified in the feature
  plan or PR description.

## Governance

This constitution supersedes informal “just ship it” shortcuts for ML and data
rules (temporal leakage, PHI, auditability). Day-to-day chore and docs changes
may proceed without a full Spec Kit cycle when they do not weaken these rules.

Amendments MUST update `.specify/memory/constitution.md` with:
1. Semantic version bump (MAJOR: remove/redefine principles; MINOR: add or
   materially expand; PATCH: clarify wording).
2. Updated **Last Amended** date (ISO `YYYY-MM-DD`).
3. A short rationale (PR description or Sync Impact Report during draft).

PRs that touch training, features, labels, serving, or explainability MUST be
reviewed for compliance with Principles II–IV and the PHI constraint. Ambiguity
is resolved in favor of the stricter interpretation until the constitution is
amended.

**Version**: 1.0.0 | **Ratified**: 2026-10-19 | **Last Amended**: 2026-10-19
