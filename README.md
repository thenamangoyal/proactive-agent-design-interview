# Proactive Agent Policy — One-Hour Design Exercise

## Goal

Design an agent that does useful work without waiting for a user prompt, while
making a deliberate decision before every consequential action:

- **Proceed** — act silently.
- **Proceed and notify** — act, then inform the user without blocking.
- **Ask** — pause for a specific user decision.
- **Escalate** — stop and route urgent or higher-authority attention.

The central problem is not simply proactivity or safety. It is the policy that
decides, for each proposed action, which one should win.

## Interview format

- You have **60 minutes**.
- You may use any AI coding or research agent.
- This is a **system-design exercise**, not a coding-speed test.
- You are not expected to build real Gmail, Calendar, payment, or background-job integrations.
- Record important assumptions and AI-assisted decisions in your design.

Spend roughly 35–40 minutes creating the design, followed by a discussion and
red-team walkthrough with the interviewer.

## Choose a concrete surface

Recommended: an inbox/calendar assistant that can triage messages, draft or send
replies, schedule meetings, detect unfinished threads, and follow up.

You may choose another surface if it still includes:

- Events that arrive without a user message
- Actions with meaningfully different consequences
- External side effects
- User corrections that should change future behavior

## What to produce

Create a `DESIGN.md` using [DESIGN_TEMPLATE.md](DESIGN_TEMPLATE.md) as a guide.
Your design should include:

1. A small architecture diagram
2. The input and output of each major component
3. A worked trace from event to final outcome
4. A sensitivity or blast-radius model
5. A four-way gating policy
6. Execution, reversibility, and outcome-verification semantics
7. A correction-learning design
8. Policy persistence, audit, versioning, and rollback
9. An evaluation plan that catches both excessive passivity and excessive autonomy
10. Explicit assumptions, tradeoffs, and what you would cut from a first version

You may include pseudocode, tables, formulas, or sketches. Working code is optional.

## Required design questions

### 1. Events and genuine proactivity

Explain how work is initiated without a new user turn. Address triggers such as
incoming messages, calendar changes, timers, or unfinished obligations. Explain
what state persists between events and how the agent avoids duplicated or stale
actions.

### 2. Action proposal

Define what the proposer receives and produces. It must be able to propose no
action, one action, or multiple actions. Explain how you represent action
arguments, confidence or ambiguity, and the source of the instruction.

### 3. Sensitivity and blast radius

Model consequence using features rather than only an action-type switch. At a
minimum, consider recipient scope, reversibility, money or commitment, private
data, novelty, provenance, and audience size. Explain how one extreme feature
can avoid being hidden by several harmless features.

### 4. Gating policy

Show how sensitivity, confidence, context, prior user preferences, and hard
constraints produce one of the four decisions. Explain why low-confidence work
does not always result in asking, and distinguish an ordinary ask from an
escalation.

### 5. Execution and reversibility

Explain what "executed" means. Cover preconditions, idempotency, safer action
variants, undo or compensation, deadlines, and verification that the external
system actually reached the intended state.

### 6. Learning from corrections

Explain how feedback such as "you didn't need to ask" and "you should have
asked" changes future policy. Address:

- How evidence is accumulated and conflicts are handled
- How learning transfers to similar actions without spreading globally
- How old evidence weakens or expires
- How the system distinguishes a gate error from a wrong intent, wrong argument,
  wrong content, or incorrectly scoped preference

Do not assume that every correction should change autonomy.

### 7. Policy governance

Explain how learned policy is logged, persisted, versioned, inspected, rolled
back, and protected from untrusted or poisoned feedback.

### 8. Evaluation

Design labeled scenarios across:

- Proactive opportunity: yes or no
- Sensitive action: yes or no

At minimum, measure:

- Missed-proactive rate
- Wrongful-autonomous rate
- Ask-fatigue rate

Also explain how you would evaluate action discovery separately from gate
quality, and how you would avoid grading a policy only on scenarios designed to
make it look successful.

## Scenario packet

Use [SCENARIOS.md](SCENARIOS.md) to demonstrate your policy. You do not need to
implement the scenarios, but your `DESIGN.md` should walk through at least three,
including one learning-across-runs example.

## What is intentionally unspecified

There is no required formula, model, framework, database, or learning algorithm.
Choose mechanisms that fit the evidence available and explain why they are
appropriate. We value clear judgment and honest limitations over unnecessary
complexity.
