# Learning Agent Stdlib

> **Source fidelity: 80%** — The stdlib concept, rule structure (description, globs, filters, actions, examples, metadata), and authoring workflow are directly sourced from ghuntley.com/stdlib/. The specific learning rules are our adaptation of this pattern.

## What is Stdlib?

From ghuntley.com/stdlib/:

> "Instead of approaching Cursor from the angle of 'implement XYZ of code' you should instead be thinking of building out a 'stdlib' (standard library) of thousands of prompting rules and then composing them together like unix pipes."

Stdlib is NOT code. It's a library of **behavioral rules** that steer agent behavior. Each rule tells the agent what to do (or not do) in specific situations.

## The Formula

```
/specs + /stdlib + loopback + evaluation = good outcomes
```

- **Specs** define WHAT the system does (see `specs/`)
- **Stdlib** defines HOW the agent behaves while doing it (this folder)
- **Loopback** means "keep going until done"
- **Evaluation** means backpressure (grading, tests)

## Rule Structure (from ghuntley.com/stdlib/)

Every rule follows this structure:

```yaml
---
description: {what the rule does}
globs: {file patterns it applies to, or *}
---

# {Rule Title}

{Description of the rule's purpose}

<rule>
  name: {rule_name}
  description: {detailed description}
  filters:
    - type: {file_extension | content | event | intent | file_name}
      pattern: {matching pattern}
  actions:
    - type: {reject | suggest | execute}
      message: |
        {what to tell the agent}
  examples:
    - input: {example trigger}
      output: {expected behavior}
  metadata:
    priority: {critical | high | medium | low}
    version: {version number}
</rule>
```

### Filter types (from the stdlib post)
- **file_extension** — match by file type
- **content** — match by content patterns
- **event** — match by events (e.g., "file_create", "build_success")
- **intent** — match by what the agent is trying to do
- **file_name** — match by filename patterns

### Action types (from the stdlib post)
- **reject** — block the behavior with an error message
- **suggest** — recommend an alternative approach
- **execute** — automatically run a command

## How to Author New Rules

From ghuntley.com/stdlib/:

1. **Observe bad behavior** — The agent does something wrong or suboptimal
2. **Ask the agent to author a rule** — "When Cursor gets it right after intervention, ask it to author a rule or update a rule with its learnings."
3. **Loop back** — "Look at the rules. What is missing? What does not follow best practice."
4. **Expect ~45% accuracy** — "The foundational LLM models right now are what I'd estimate to be at circa 45% accuracy and require frequent steering."
5. **Exponential growth** — "Improvements come slowly in the beginning, but your gains increase rapidly over time."

## Current Rules

| Rule | Purpose |
|------|---------|
| `explanation-format.md` | How to format explanations for the learner |
| `handle-uncertainty.md` | How to handle "I don't know" responses |
| `verify-before-teaching.md` | Always verify claims before teaching |
| `break-down-weak-topics.md` | Break topics into sub-topics when score is low |

## Growing the Stdlib

This folder should grow over time as you observe agent behavior:
- Agent gives a confusing explanation? Author a rule for explanation clarity.
- Agent skips verification? Author a rule to enforce it.
- Agent asks too many easy questions? Author a rule for difficulty calibration.
- Agent gives away answers too quickly? Author a rule for Socratic patience.

The key insight from the stdlib post: **you are building a library of behavioral constraints that compounds over time.**
