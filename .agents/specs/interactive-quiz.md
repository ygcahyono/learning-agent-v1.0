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

<!-- DECIDED 2026-03-23:
- 3-5 questions per session (confirmed).
- Focus mode: Agent decides based on progress — deep dive on weak topics,
  mixed when things are balanced.
- Follow-up probing: Aggressive on weak areas, lighter on strong areas.
- Hints: Give hints after a pause. If learner says "I don't know" or seems
  stuck, offer a hint before revealing the answer. -->

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

### 7. Save Session Transcript
<!-- DECIDED 2026-03-24: Save full Q&A to sessions/ folder. -->
- Save the full Q&A transcript to `sessions/{topic-slug}-{YYYY-MM-DD}.md`
- Use kebab-case for topic slug (e.g., `linear-algebra-basics-2026-03-24.md`)
- File format:

```markdown
# Session: {Topic Name} — Session {N}
Date: {YYYY-MM-DD} | Type: Interactive | Score: {TBD or final %}

---

## Q1
**Agent:** {question as asked}

**You:** {learner's answer verbatim}

**Feedback:** {agent's evaluation and expansion}

---

## Q2
...

---

## Notes
- {key observations about learner's understanding}
- {gaps identified}
- {next session plan}
```

- Save after **every Q&A exchange** — do not wait until end of session
- Do not alter the learner's answer — record it verbatim

### 8. Update Progress
- Update `progress.md` per the progress-tracking spec

## Question Types

<!-- DECIDED 2026-03-23: Keep current question categories. Start loose, evolve
via stdlib — when bad questions are observed, author rules to improve them. -->

**Conceptual:** "Explain how [concept] works and why it matters."
**Diagnostic:** "A model is showing [symptom, e.g., high bias, overfitting]. Walk through your diagnosis."
**Comparison:** "What's the difference between [A] and [B]? When would you use each?"
**Design:** "How would you design a pipeline for [task, e.g., classifying customer churn]?"
**Deep Dive:** "You mentioned [concept]. What happens mathematically when [scenario]?"

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

<!-- DECIDED 2026-03-23:
- Session length: 3-5 questions. Short, focused sessions.
- Session cadence: As often as needed (loopback pattern).
- Mixed vs focused: Agent decides based on progress data.
- "I don't know" handling: See stdlib/handle-uncertainty.md — give hints
  after a pause before revealing the answer. -->
