<!--
Sync Impact Report

Version change: TEMPLATE -> 1.0.0
Modified principles:
- [PRINCIPLE_1_NAME] -> Test-First Development (NON-NEGOTIABLE)
- [PRINCIPLE_2_NAME] -> Small, Iterative Deliveries
- [PRINCIPLE_3_NAME] -> Defendable Simplicity (YAGNI)
- [PRINCIPLE_4_NAME] -> Branching, Reviews & Merge Gates
- [PRINCIPLE_5_NAME] -> Quality, Observability & Versioning
Added sections:
- Development Workflow & Acceptance
- Quality Gates & Compliance
Removed sections:
- None
Templates requiring updates:
- .specify/templates/plan-template.md ✅ updated
- .specify/templates/spec-template.md ✅ reviewed (no change required)
- .specify/templates/tasks-template.md ✅ updated
- .specify/templates/checklist-template.md ⚠ pending (review for PR checklist alignment)
Follow-up TODOs:
- TODO(RATIFICATION_DATE): Confirm original ratification date if different from 2025-11-08
-->

# nidc-2025-logic-grid-puzzles Constitution

## Core Principles

### Test-First Development (NON-NEGOTIABLE)
Every change starts with a failing test: write the test (contract or integration first), confirm it fails,
implement the minimal code to pass, then refactor (Red → Green → Refactor). Tests must be
documented and included in the feature branch. When real dependencies are infeasible, document any
mocking or emulation used and the rationale for substituting real systems.

### Small, Iterative Deliveries
Deliver in small, focused increments. Use short-lived feature branches with a single logical change
per branch. Prefer frequent, reviewable merges that minimize blast radius and simplify rollbacks.

### Defendable Simplicity (YAGNI)
Favor the smallest change that solves the problem. Avoid premature abstractions and cleverness.
When adding complexity, require a short justification: why it's necessary and which simpler
alternatives were considered and rejected.

### Branching, Reviews & Merge Gates
Never push directly to `main`. Use branch prefixes: `feature/`, `fix/`, `chore/`, `perf/`, etc.
Each branch MUST represent one logical change and include the following in the PR:
- concise description and link to spec/plan
- tests demonstrating the change
- run instructions for reviewers
- a short constitutional compliance note stating which principles were followed or any
	deviations (see Governance for exception handling)
Require at least one reviewer (two for cross-team or high-risk changes). Do not bypass checks
unless a documented rationale and remediation plan are included in the PR.

### Quality, Observability & Versioning
Encourage pre-commit hooks, linters, and type checks where available; require documented
fallbacks when tooling isn't available. Prefer structured logs, metrics, and traces. Follow
semantic versioning for public artifacts. If centralized tooling is unavailable, require local
equivalents and documentation describing how to reproduce checks locally.

## Development Workflow & Acceptance

Merges require:
- passing automated checks available in CI (lint, tests, type checks)
- updated documentation and run instructions
- reviewer sign-off

If a deviation is approved (for example, temporary test bypass), the PR MUST include a written
rationale, proposed remediation steps, and an explicit schedule for follow-up.

Acceptance criteria for features must be expressed as testable scenarios in the feature spec and
linked from the PR. Complex or cross-cutting changes must include a short migration/rollback plan.

## Quality Gates & Compliance

Constitutional compliance is checked at plan and PR stages. Plans must include a short
"Constitution Check" section explaining how the work satisfies the principles listed above and
highlighting any anticipated violations with justification. Any non-trivial violation must be
approved by a maintainer and include an implementation and remediation plan.

## Governance

Amendments: Changes to this Constitution MUST be proposed via a spec and pull request that:
1. States the proposed change and rationale.
2. Explains impact on existing practices and templates.
3. Includes a migration or deprecation plan for affected artifacts.

Approval: A constitution amendment requires at least two approvers from active maintainers and
passing checks where practical. Major governance or principle redefinitions require broader
announcement to the team.

Versioning policy:
- MAJOR: Backward-incompatible governance or principle removals/redefinitions.
- MINOR: New principle or material expansion of guidance.
- PATCH: Wording, clarification, or non-semantic fixes.

Compliance reviews: Periodic reviews SHOULD be scheduled (annual suggested) to ensure the
constitution still fits the team's needs. Ad-hoc compliance reviews may be triggered by
significant process or tooling changes.

**Version**: 1.0.0 | **Ratified**: 2025-11-08 | **Last Amended**: 2025-11-08

