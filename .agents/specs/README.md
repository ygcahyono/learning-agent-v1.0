# Learning Agent Specifications

> **Source fidelity: 70%** — The Ralph-to-learning mapping is grounded in accessible Huntley content. Usage instructions and system overview are my interpretation. Could not access the paywalled specs post to see how Huntley introduces a specs folder.

## Overview

This `.agents/` folder contains the rules and specifications that drive the learning system.

## Core Principles (from Huntley's accessible posts)

These are directly sourced from the Ralph post (ghuntley.com/ralph/) and the Ralph Playbook repo:

1. **Specs drive behavior** — Don't ask the agent to "teach me." Define specifications for how learning should work. The agent follows the specs.
2. **One task per loop** — Each interaction focuses on one learning unit.
3. **Backpressure is everything** — In coding: tests and compilers. In learning: quizzes and grading. The agent loops until competence is demonstrated.
4. **Progress tracking as state** — `progress.md` persists between sessions, like `IMPLEMENTATION_PLAN.md` in Ralph.
5. **Let the agent decide what's important** — The agent reads progress, identifies weaknesses, chooses the next topic.
6. **Deterministic context loading** — Every session loads the same specs and progress file.
7. **Capture the why** — When grading, capture WHY an answer was wrong, not just that it was wrong.
8. **No placeholders** — Don't accept surface-level answers. Push for depth.
9. **Plan is disposable** — If progress tracking gets stale, regenerate it.

## File Map

```
.agents/
├── rules/
│   └── timestamp-generation.md    # How to name generated files
├── specs/
│   ├── README.md                  # This file
│   ├── grading-workflow.md        # How to grade responses (backpressure)
│   ├── interactive-quiz.md        # Real-time Socratic Q&A format
│   ├── practice-labs.md           # Hands-on exercise generation
│   ├── progress-tracking.md       # Persistent learner state
│   ├── quiz-workflow.md           # Written quiz generation & adaptation
│   └── technical-verification.md  # Accuracy checking
```

## Concept Mapping

| Ralph (Coding) — sourced from ghuntley.com/ralph/ | Learning Agent |
|----------------------------------------------------|----------------|
| `specs/*.md` (requirements) | Learning workflow specifications |
| `IMPLEMENTATION_PLAN.md` (task state) | `progress.md` (learner state) |
| `AGENTS.md` (how to build/run) | Operational notes on study setup |
| Backpressure (tests, build, typecheck) | Backpressure (grading, quizzes) |
| One task per loop | One topic per session |
| Subagents for file search | Subagents for web research |
| `fix_plan.md` (prioritized TODO) | Study plan / weakness list |
| "Don't assume not implemented" | "Don't assume learner doesn't know" |
| "No placeholder implementations" | "No surface-level answers" |
| "Capture the why" in test docs | "Capture the why" in grading feedback |

## The Learning Loop

```
while learning:
    1. Agent reads specs/* (how learning works)
    2. Agent reads progress.md (current learner state)
    3. Agent selects the most important topic (weakest area)
    4. Agent conducts session (quiz / interactive / practice)
    5. Agent grades responses (backpressure)
    6. Agent updates progress.md
    7. Session ends → next session starts fresh with updated state
```

<!-- YOUR INPUT NEEDED
The 30% gap:

- I could not access ghuntley.com/specs/ (paywalled) to see how Huntley
  introduces and structures a specs folder. The current README format
  is my interpretation.

- I could not access ghuntley.com/stdlib/ (paywalled) to understand the
  "standard library" concept fully. There may be a stdlib/ equivalent
  for learning (e.g., reusable patterns for "how to format explanations"
  or "how to structure a reading document"). If you get access to the
  stdlib post, consider adding a stdlib/ folder.

- The loop description above is my adaptation of the Ralph loop.
  The original author may have structured their learning loop differently.

- If you want to add a PROMPT.md (the actual text piped into Claude Code
  each session) and/or a loop.sh launcher script, they would live at the
  repo root alongside progress.md. Templates exist in the Ralph Playbook
  repo: https://github.com/ghuntley/how-to-ralph-wiggum
-->
