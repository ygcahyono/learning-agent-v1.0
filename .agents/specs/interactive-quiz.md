# Spec: Interactive Quiz

> **Source fidelity: 55%** — The X post author explicitly described this format: "the agent asks a question and lets me try to answer with my current existing knowledge and then expand & correct it, kinda like a personal tutor." That confirms WHAT this file should do. The loopback pattern from ghuntley.com/specs/ ("keep going until implemented" = keep quizzing until understood) now directly grounds the iterative approach. The HOW (specific workflow steps, question types, anti-patterns) is our extrapolation using Huntley's principles.

## Purpose

The interactive quiz is the real-time, Socratic learning mechanism. The agent asks, the learner answers from existing knowledge, the agent evaluates and expands. This is the primary mode of learning.

## What's Confirmed (from the X post)

The original author described:
- Agent asks a question
- Learner tries to answer with current existing knowledge
- Agent expands and corrects the answer
- It's "kinda like a personal tutor"
- It tracks progress so it knows what the learner already knows and what weaknesses exist

## The Loopback Applied to Interactive Learning

From ghuntley.com/specs/, the loopback is "keep going until implemented" — restart context, same prompt, repeatedly. Applied to interactive quizzes, this becomes: **keep quizzing until understood**. Each session is a loop iteration. The agent reads progress, asks questions, evaluates, updates progress, and the next session picks up where this one left off.

The key loopback prompt from the specs post:
> "Study @SPECS.md for functional specifications. Implement what is not implemented. Create tests. Run build and verify."

Adapted: "Study @specs/README.md. Read @progress.md. Quiz on what is not yet understood. Grade and verify understanding."

## Workflow

<!-- YOUR INPUT NEEDED
The workflow below is our design applying Huntley's principles.

Key decisions you should validate:
- Number of questions per session (we suggest 3-5)
- Whether to go deep on one topic vs. broad across several
- How aggressive the follow-up probing should be
- Whether the agent should give hints or let the learner struggle
-->

### 1. Orient
- Read `progress.md` for current learner state
- Read `readings/*.md` and `questions/*.md` for topic domain
- Identify weakest areas from progress data

### 2. Select Topic
- Choose most important topic based on:
  - Areas with lowest scores
  - Topics not yet covered
  - Topics with partial understanding (most growth potential)
- **Do not assume the learner doesn't know something** — probe first
  (From Huntley: "don't assume not implemented")

### 3. Ask a Question
- Open-ended question testing understanding, not recall
- Prefer "explain how" and "why" over "what is"
- Frame in practical/interview contexts where possible

### 4. Wait for Learner's Answer
- **Do not provide the answer immediately**
- The learning happens in the attempt

### 5. Evaluate and Expand
After the learner responds:
- Acknowledge what was correct
- Identify gaps or misconceptions (be specific)
- Expand with the correct/complete explanation
- Connect to related concepts
- Capture the why (Huntley principle)

### 6. Follow-Up
- Weak answer → ask simpler follow-up to build incrementally
- Strong answer → probe deeper or move to adjacent topic

### 7. Update Progress
- Update `progress.md` per the progress-tracking spec

## Question Types

<!-- YOUR INPUT NEEDED
These question categories are our design. You may want to:
- Add domain-specific question types for your topics
- Remove categories that don't apply
- Let the agent evolve its own question types over time
- Author stdlib rules for question formatting (following Huntley's
  pattern: observe bad questions → ask agent to write a rule)
-->

**Conceptual:** "Explain how [concept] works and why it matters."
**Troubleshooting:** "A system is showing [symptom]. Walk through your diagnosis."
**Comparison:** "What's the difference between [A] and [B]? When would you use each?"
**Design:** "How would you set up [system] for [requirement]?"
**Deep Dive:** "You mentioned [concept]. What happens under the hood when [scenario]?"

## Anti-Patterns

These follow from Huntley's principles:

- **Do not lecture** — Ask, don't tell. Teaching through correction, not monologue.
  (From Ralph: the agent does the work, backpressure validates it)
- **Do not accept surface-level answers** — Push for specifics.
  (From Huntley: "no placeholder implementations")
- **Do not skip to next topic too quickly** — Stay and approach from different angle.
  (From Huntley: "one task per loop" — finish before moving on)
- **Do not ask yes/no questions** — These don't test understanding.
- **Do not provide hints too early** — Let the learner sit with not knowing.

<!-- YOUR INPUT NEEDED
The following aspects are open for your design. Note that the
loopback pattern means sessions are cheap — you can always run
another one. "Keep going until implemented" = keep going until
understood. So err on the side of shorter, more frequent sessions.

- Session length: How many questions before ending? We suggest 3-5.
- Session cadence: How often? The loopback pattern says: as often
  as needed. "Did the LLM go on a bad path? Restart a new chat
  session... Keep doing it until everything is implemented."
- Mixed vs. focused: One topic deeply or touch multiple?
- How to handle "I don't know": See stdlib/handle-uncertainty.md
-->
