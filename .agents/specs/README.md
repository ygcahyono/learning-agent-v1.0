# Learning Agent Specifications

> **Source fidelity: 85%** — The Ralph-to-learning mapping is grounded in accessible Huntley content. The specs and stdlib PDFs (now accessible) confirm: specs are functional specifications one per domain topic, with a SPECS.md overview linking to all specs. Stdlib is a library of prompting rules composed like unix pipes. The formula `/specs + /stdlib + loopback + evaluation` is now directly sourced. Remaining 15% gap: specific adaptation of these patterns to learning (vs. coding) is still our interpretation.

## Overview

This `.agents/` folder contains the rules and specifications that drive the learning system.

## The Formula (directly sourced from ghuntley.com/specs/)

```
/specs + /stdlib + loopback + evaluation = good outcomes
```

This is Huntley's core formula for agent-driven work. Applied to learning:

- **`/specs`** — Functional specifications, one per domain topic. This folder. Each spec defines HOW a part of the learning system works. (Source: ghuntley.com/specs/ — "Write up the specifications into the specs/ folder with each domain topic as a separate markdown file.")
- **`/stdlib`** — A library of behavioral rules that steer agent behavior, composed like unix pipes. See `.agents/stdlib/`. Each rule has filters, actions, examples, and metadata. (Source: ghuntley.com/stdlib/ — "building out a stdlib of thousands of prompting rules and then composing them together like unix pipes.")
- **`loopback`** — The key workflow. Restart context, same prompt, repeatedly. "Keep going until implemented." (Source: ghuntley.com/specs/ — "Study @SPECS.md for functional specifications. Implement what is not implemented. Create tests. Run build and verify.")
- **`evaluation`** — Backpressure. Tests/build in coding; grading/quizzes in learning. Automated feedback that rejects invalid output. (Source: banay.me — "Back pressure helps the agent identify mistakes as it progresses.")

## Core Principles (from Huntley's posts)

These are directly sourced from the Ralph post, the Ralph Playbook repo, and the specs/stdlib posts:

1. **Specs drive behavior** — Don't ask the agent to "teach me." Define specifications for how learning should work. The agent follows the specs. (Confirmed by specs post: "Specifications are the heart of your application; the internal implementation matters less.")
2. **One task per loop** — Each interaction focuses on one learning unit.
3. **Backpressure is everything** — In coding: tests and compilers. In learning: quizzes and grading. The agent loops until competence is demonstrated.
4. **Progress tracking as state** — `progress.md` persists between sessions, like `IMPLEMENTATION_PLAN.md` in Ralph.
5. **Let the agent decide what's important** — The agent reads progress, identifies weaknesses, chooses the next topic.
6. **Deterministic context loading** — Every session loads the same specs and progress file.
7. **Capture the why** — When grading, capture WHY an answer was wrong, not just that it was wrong.
8. **No placeholders** — Don't accept surface-level answers. Push for depth.
9. **Plan is disposable** — If progress tracking gets stale, regenerate it.
10. **The LLM writes the rules** — Observe bad agent behavior, ask the LLM to author a rule to prevent it. (Source: ghuntley.com/stdlib/ — "When Cursor gets it right after intervention, ask it to author a rule or update a rule with its learnings.")

## File Map

```
.agents/
├── rules/
│   └── timestamp-generation.md    # How to name generated files
├── specs/
│   ├── README.md                  # This file (the SPECS.md overview)
│   ├── grading-workflow.md        # How to grade responses (backpressure/evaluation)
│   ├── interactive-quiz.md        # Real-time Socratic Q&A format
│   ├── practice-labs.md           # Hands-on exercise generation
│   ├── progress-tracking.md       # Persistent learner state
│   ├── quiz-workflow.md           # Written quiz generation & adaptation
│   └── technical-verification.md  # Accuracy checking
├── stdlib/
│   ├── README.md                  # Stdlib overview and rule authoring guide
│   ├── explanation-format.md      # How to format explanations
│   ├── handle-uncertainty.md      # How to handle "I don't know" responses
│   ├── verify-before-teaching.md  # Always verify claims before teaching
│   └── break-down-weak-topics.md  # Break topics into sub-topics when score is low
```

## Concept Mapping

| Ralph (Coding) | Learning Agent | Source |
|----------------|----------------|--------|
| `specs/*.md` (requirements) | Learning workflow specifications | ghuntley.com/specs/ |
| `SPECS.md` (overview linking all specs) | This README (overview of all learning specs) | ghuntley.com/specs/ |
| `stdlib/` (behavioral rules for agent) | `.agents/stdlib/` (behavioral rules for learning agent) | ghuntley.com/stdlib/ |
| `IMPLEMENTATION_PLAN.md` (task state) | `progress.md` (learner state) | ghuntley.com/ralph/ |
| `AGENTS.md` (how to build/run) | Operational notes on study setup | Ralph Playbook repo |
| Backpressure (tests, build, typecheck) | Backpressure (grading, quizzes) | banay.me, ghuntley.com/pressure/ |
| Loopback ("keep going until implemented") | Loopback ("keep quizzing until understood") | ghuntley.com/specs/ |
| One task per loop | One topic per session | ghuntley.com/ralph/ |
| Subagents for file search | Subagents for web research | ghuntley.com/ralph/ |
| `fix_plan.md` (prioritized TODO) | Study plan / weakness list | ghuntley.com/ralph/ |
| "Don't assume not implemented" | "Don't assume learner doesn't know" | ghuntley.com/ralph/ |
| "No placeholder implementations" | "No surface-level answers" | ghuntley.com/ralph/ |
| "Capture the why" in test docs | "Capture the why" in grading feedback | ghuntley.com/ralph/ |
| "Look at rules. What's missing?" | "Look at learning rules. What's missing?" | ghuntley.com/stdlib/ |
| Rules authored by the LLM itself | Learning rules authored by the agent | ghuntley.com/stdlib/ |

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
The 15% gap:

- The learning loop above is our adaptation of the Ralph loop applied to
  learning. The original X post author may have structured their learning
  loop differently. The loop mechanics are now well-grounded in Huntley's
  posts, but the specific application to learning is our interpretation.

- If you want to add a PROMPT.md (the actual text piped into Claude Code
  each session) and/or a loop.sh launcher script, they would live at the
  repo root alongside progress.md. Templates exist in the Ralph Playbook
  repo: https://github.com/ghuntley/how-to-ralph-wiggum

  The loopback prompt from the specs post is:
  "Study @SPECS.md for functional specifications. Study @.cursor for
  technical requirements. Implement what is not implemented. Create tests.
  Run build and verify."

  Adapted to learning, this might be:
  "Study @specs/README.md for learning specifications. Study @progress.md
  for current learner state. Assess what is not yet understood. Generate
  quiz. Grade and verify understanding."
-->
