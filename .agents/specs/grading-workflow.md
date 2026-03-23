# Spec: Grading Workflow

> **Source fidelity: 65%** — Backpressure philosophy is well-documented in accessible Huntley posts. "Capture the why," "no placeholders," and "backpressure rejects invalid output" are directly sourced. Grading is now grounded as the "evaluation" component of the `/specs + /stdlib + loopback + evaluation` formula (sourced from ghuntley.com/specs/). The 1-5 scoring scale, specific grading process steps, and feedback format are my design.

## Purpose

Grading is the **evaluation** component of the formula `/specs + /stdlib + loopback + evaluation`. Without grading, there is no feedback signal, and without feedback, the learning loop cannot adapt.

## Why This File Exists

In Huntley's system, backpressure is what prevents the agent from producing garbage. Tests reject invalid code. The formula from ghuntley.com/specs/ makes evaluation explicit: it's one of the four pillars. For coding, evaluation = `cargo test`, `cargo build`, `cargo clippy`. For learning, evaluation = grading and quizzes.

From ghuntley.com/specs/: "The secret to hands-free vibe coding is really just this prompt when used in conjunction with stdlib and specs library... Study @SPECS.md for functional specifications. Implement what is not implemented. Create tests. Run a 'cargo build' and verify the application works."

Applied to learning: the "create tests and verify" step IS grading. Grading rejects surface-level understanding the same way `cargo test` rejects broken code.

**Directly sourced principles:**

From ghuntley.com/ralph/:
- "DO NOT IMPLEMENT PLACEHOLDER OR SIMPLE IMPLEMENTATIONS. WE WANT FULL IMPLEMENTATIONS."
  → Learning equivalent: do not accept placeholder answers. Push for full understanding.

- "Important: When authoring documentation, capture the why — tests and implementation importance."
  → Learning equivalent: when grading, capture WHY an answer was wrong.

- "If functionality is missing then it's your job to add it as per the application specifications."
  → Learning equivalent: if understanding is missing, the agent must address it, not skip it.

From banay.me (search snippets):
- "Back pressure helps the agent identify mistakes as it progresses"
- "You should only be rejecting work based on high-level architecture, business logic, or user experience issues. If you are rejecting work because of syntax errors or failing tests, you are wasting your back pressure."
  → Learning equivalent: grade on understanding depth, not keyword presence.

From ghuntley.com/pressure/ (partial access):
- "Software engineering is now about preventing failure scenarios and preventing the wheel from turning over through back pressure to the generative function."

## Grading Principles (sourced)

### 1. No Placeholder Answers
Directly from Huntley's "no placeholder implementations" rule.

- "It's for performance" → Not accepted. WHY? HOW?
- "It's a best practice" → Not accepted. What problem does it solve?
- "It depends" → Only accepted if followed by specific conditions.

### 2. Capture the Why
Directly from Huntley's "capture the why" principle.

When grading, always document:
- What was correct
- What was incorrect
- Why the correct answer matters
- What to study next

### 3. Backpressure Must Reject Invalid Output
From the backpressure philosophy: a grade that doesn't reflect reality is wasted backpressure.

## Grading Scale

<!-- YOUR INPUT NEEDED
The 1-5 scale below is our design. Huntley's backpressure is binary
(tests pass or fail, build compiles or doesn't). However, the stdlib
post reveals that rules use actions like "reject" and "suggest" —
not just pass/fail — so a more granular scale is reasonable.

Options:
- Keep the 1-5 scale (more granular, better for tracking trends)
- Use pass/fail (simpler, closer to Huntley's original binary backpressure)
- Let the agent decide the scale (Huntley: "let Ralph dictate format")
-->

### Score: 1 — No Understanding
- Answer is wrong, blank, or off-topic
- Action: Flag for priority study. Break into simpler sub-components.

### Score: 2 — Minimal Understanding
- Vague awareness but significant misconceptions
- Action: Revisit foundational readings. Tier 1 questions.

### Score: 3 — Partial Understanding
- Core idea captured but incomplete. Missing details or edge cases.
- Action: Target with Tier 2-3 questions. Probe specific gaps.

### Score: 4 — Good Understanding
- Substantially correct and well-reasoned. Minor gaps.
- Action: Move to Tier 3-4 questions.

### Score: 5 — Expert Understanding
- Thorough, nuanced, covers edge cases and trade-offs.
- Action: Topic approaching mastery. Retention checks only.

## Grading Process

<!-- YOUR INPUT NEEDED
The step-by-step process below is our design. Adjust as needed.
The stdlib rule structure (filters → actions → examples) suggests
grading could also be structured as rules — e.g., a "grading" stdlib
rule that rejects surface-level answers automatically.
-->

1. **Read the question and model answer** — Reference readings for source material
2. **Read the learner's response** — Read fully before judging
3. **Identify correct elements** — Be specific
4. **Identify gaps and errors** — Be specific
5. **Assign score** — Use the scale above
6. **Write feedback** — Structure as:

```markdown
### Grade: {score}/5

**What you got right:**
{specific accurate elements}

**What was missing or incorrect:**
{specific gaps}

**Why this matters:**
{why the correct answer is important}

**To improve:**
{specific readings or concepts to revisit}
```

7. **Update progress.md** — Feed grades into progress tracking

## Anti-Patterns (sourced from Huntley's philosophy)

- **Do not grade generously** — Wasted backpressure. From banay.me: "you're going to be stuck spending your time telling the agent about each mistake."
- **Do not grade on keyword presence** — Grade on understanding depth.
- **Do not skip feedback** — A score without feedback is useless. "Capture the why."
