# Design a Proactive Agent That Knows When to Ask

## Interview Format

This is a **50-minute live system-design discussion**, not a coding exercise.

- Use Excalidraw or another whiteboard to draw your design.
- Working code is not expected.
- The interviewer may clarify requirements or introduce new scenarios.
- Build and explain the design interactively rather than working silently and
  presenting only at the end.
- Your final artifact should be one understandable architecture diagram with
  enough annotations to support the discussion.

## AI Use

You may use any AI coding, research, or diagramming agent. Keep your AI work
visible while sharing your screen and briefly narrate how you are using it.

AI use is not judged negatively. However, you are responsible for the final
design and must be able to explain, challenge, and modify every part of it
without relying on the agent to answer for you. During the discussion, the
interviewer may ask you to reason through a new scenario before consulting AI.

At the end, be prepared to identify one AI suggestion you accepted and one you
rejected or materially changed.

## Problem

Design an agent that is genuinely proactive: it surfaces next moves the user did
not request, works between user turns, and closes unfinished loops. At the same
time, it must know when to stop and involve a human.

Use an inbox/calendar assistant as the concrete surface. It may triage messages,
draft or send replies, schedule meetings, detect unfinished threads, follow up,
and react to a stream of inbox, calendar, timer, and user events.

For every proposed action, the system must choose one of four outcomes:

- **Proceed** — act silently.
- **Proceed and notify** — act, then inform the user without blocking.
- **Ask** — pause for a specific user decision.
- **Escalate** — stop and route urgent or higher-authority attention.

The central problem is not proactivity or safety in isolation. It is the policy
that decides, for each proposed action, which one should win. Over-asking is a
failure mode, not a safe default.

## Core Requirements

### Proactivity and Event Loop

The agent must do more than respond to direct user messages. It should receive a
stream of inbox, calendar, timer, or state-change events; identify useful next
moves the user did not request; and continue tracking unfinished work across
turns.

Your design should explain:

- What creates an event or future obligation
- What context and state are loaded when it fires
- How the agent proposes zero, one, or multiple candidate actions
- How it avoids duplicate, stale, or no-longer-needed actions
- How it records and verifies that a loop was actually closed

```text
Inbox / calendar / timer / unfinished obligation
                         |
                         v
                    Event intake
                         |
                         v
              Context and current-state check
                         |
                         v
        Zero or more useful proposed next actions
                         |
                         v
              Sensitivity model and gate
```

Proactivity describes **how an action is discovered**, not whether it is safe to
execute autonomously. A proactive action may still Proceed, Proceed and notify,
Ask, or Escalate depending on its sensitivity and context.

### Sensitivity Model

The sensitivity model answers: **If this proposed action is wrong, how large is
the potential consequence?** We call that consequence its **blast radius**.

Blast radius is not the model's confidence and is not the probability that harm
will occur. It describes the size and reach of a possible mistake. For example:

| Proposed action | Possible consequence if wrong | Example blast radius |
|---|---|---|
| Archive a restorable newsletter | Small and easy to undo | Low |
| Move an internal team meeting | Affects several people but can be repaired | Medium |
| Send private employee data externally | Serious privacy and organizational impact | High |

Classify blast radius using features rather than only an action-type switch. At
minimum, consider recipient, reversibility, money or commitment, and novelty.
Your design should explain how these features combine and what the sensitivity
model returns for the gate to consume.

### Gating Policy

The gating policy answers a different question: **Given the potential
consequence and what we know about this situation, how much autonomy should the
agent receive?**

| Component | Main question | Example inputs | Output |
|---|---|---|---|
| Sensitivity model | How consequential would a mistake be? | Proposed action, recipients, reversibility, money, novelty | Sensitivity features and blast radius |
| Gating policy | How much autonomy is appropriate now? | Blast radius, confidence, context, preferences, prior evidence, hard constraints | Proceed, Proceed and notify, Ask, or Escalate |

The relationship should look roughly like this, although you should design the
actual interfaces and decision mechanism:

```text
Proposed action + action context
                |
                v
        Sensitivity model
                |
                v
     Sensitivity / blast-radius report
                |
                |  + confidence + current context
                |  + user preferences and learned policy
                |  + hard constraints
                v
           Gating policy
                |
                v
  Proceed | Proceed and notify | Ask | Escalate
```

Make the decision process understandable and testable. Explain why two actions
with the same blast radius might receive different decisions because their
confidence, context, provenance, or prior user authorization differs.

### Learning from Corrections

When the user says “you didn’t need to ask,” future policy should move toward
appropriate autonomy. When the user says “you should have asked,” it should move
toward appropriate caution. Explain how the correction affects later, similar
actions without creating unsafe global behavior.

### Evaluation

Design labeled scenarios across both axes:

- Proactive opportunity: yes or no
- Sensitive action: yes or no

At minimum, measure missed-proactive rate, wrongful-autonomous rate, and
ask-fatigue rate.

## What to Show

Your diagram and explanation should make the end-to-end system understandable:

1. What triggers the agent without a user turn
2. The major components and the input/output of each
3. How an event becomes zero or more proposed actions
4. How sensitivity and the gate produce a decision
5. What happens after each of the four decisions
6. How a user correction changes a later decision
7. What state must persist across runs
8. How you would evaluate whether the design is both useful and safe

Walk through at least one concrete scenario, identify important failure modes,
and state what you would intentionally leave out of a first version.

There is no required formula, model, framework, database, or learning algorithm.
We value clear interfaces, justified tradeoffs, failure awareness, and your own
reasoning more than unnecessary complexity or polished AI-generated output.
