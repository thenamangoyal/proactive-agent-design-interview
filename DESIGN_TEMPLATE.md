# Candidate Design

## 1. Chosen Surface and Assumptions

What environment does the agent operate in? What user, organization, and tool
capabilities do you assume?

## 2. Architecture

Add a diagram and identify the major components.

## 3. Component Interfaces

For each component, state its input, output, and responsibility. At minimum,
cover event intake, proposal, sensitivity, gate, execution, correction handling,
and policy state.

## 4. End-to-End Trace

Walk through one event from trigger to verified outcome. Show what happens when
the agent proceeds and what happens when it pauses.

## 5. Sensitivity Model

List the features, their interpretation, how they combine, and how extreme
single-feature risks are handled.

## 6. Gating Policy

Define the four decisions and the policy that selects among them. Include hard
constraints and at least one decision table, formula, or equivalent mechanism.

## 7. Execution and Reversibility

Explain safer action variants, preconditions, idempotency, undo or compensation,
and outcome verification.

## 8. Learning from Corrections

Explain correction attribution, evidence tracking, scoped generalization,
conflicting evidence, aging, and how a correction changes a later decision.

## 9. Policy State and Governance

Explain audit records, persistence, policy versions, rollback, and protection
against untrusted feedback.

## 10. Evaluation

Describe scenario labels, action matching, metrics, holdouts, broken-policy
baselines, and how you would interpret results at small sample sizes.

## 11. Tradeoffs and First-Version Scope

What would you build first? What would you intentionally leave simulated? What
would have to change before real users or real external tools were introduced?

## 12. AI Assistance

Briefly note which AI tools you used and one suggestion you accepted, rejected,
or materially changed.
