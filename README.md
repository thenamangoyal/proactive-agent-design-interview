# Design a Proactive Agent That Knows When to Ask

## Interview Format

This is a **50-minute live system-design discussion**, not a coding exercise.

- Use Excalidraw or another whiteboard to develop your design.
- Working code is not expected.
- Explain important decisions as you make them; the interviewer may introduce a new scenario.

You may use any AI coding, research, or diagramming agent while sharing your
screen. AI use is not judged negatively, but you must be able to explain,
challenge, and modify every part of the resulting design. At the end, be ready
to identify one AI suggestion you accepted and one you rejected or changed.

## The Problem

Design an agent that is genuinely proactive: it identifies useful next moves the
user did not explicitly request, can work between user turns, and closes
unfinished loops. It must also know when to act independently and when to
involve a human.

Choose a concrete product surface. An inbox/calendar assistant is one option,
but you may choose another surface if it makes the tradeoffs equally clear.

The agent receives events from its environment and may propose no action, one
action, or several actions. For each proposed action, it must choose:

- **Proceed** — act without interrupting the user.
- **Proceed and notify** — act, then inform the user.
- **Ask** — pause for a specific user decision.
- **Escalate** — stop and seek urgent or higher-authority attention.

The central design problem is the policy that balances useful initiative with
the consequences of acting incorrectly. Asking too often is also a failure.

## What Your Design Must Address

### Proactive Behavior

Explain how the agent notices opportunities without a new user message, keeps
track of unfinished work, and determines whether work has actually been
completed. The triggering and persistence mechanisms are your choice.

### Sensitivity and Gating

For an action, **sensitivity** or **blast radius** means the potential consequence
if that action is wrong. It is different from confidence that the action is
correct.

For example, mistakenly archiving a restorable newsletter has a smaller blast
radius than sending private employee data to an external recipient.

Design a way to assess sensitivity from the action and its context, then use it
with other relevant information to choose one of the four outcomes above. Avoid
treating every action of the same type as equally sensitive. Make the decision
understandable and testable; the representation, formula, and policy structure
are up to you.

### Learning from Corrections

If a user says, “You didn’t need to ask,” the agent should be able to become
appropriately more autonomous. If the user says, “You should have asked,” it
should become appropriately more cautious. Explain what changes, what persists
across runs, and how learning affects relevant future situations without
silently changing unrelated behavior.

### Evaluation

Explain how you would test both useful proactivity and safe autonomy. Include
scenarios covering:

- A proactive opportunity exists or does not exist.
- A proposed action is sensitive or not sensitive.

At minimum, define how you would measure:

- **Missed-proactive rate** — useful opportunities the agent misses.
- **Wrongful-autonomous rate** — actions taken autonomously when it should have paused.
- **Ask-fatigue rate** — unnecessary requests for user approval.

## What to Present

Use your diagram to show the important parts of your system and walk through one
scenario from trigger to outcome. Explain major tradeoffs and failure modes,
including what you would intentionally leave out of a first version.

There is no required architecture, formula, model, framework, database, tool
set, or learning algorithm. We care about clear reasoning, coherent interfaces,
testable decisions, and how well you adapt the design when assumptions change.
