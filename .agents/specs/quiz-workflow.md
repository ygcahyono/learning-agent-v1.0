# Spec: Quiz Workflow

> **Source fidelity: 60%** — The X post confirms: quizzes are mostly open-ended, they get graded, and the system adapts based on grades. The loopback pattern from ghuntley.com/specs/ ("keep going until implemented") now directly grounds the iterative quiz loop. Evaluation is one of the four pillars of the formula `/specs + /stdlib + loopback + evaluation`. The specific generation rules, difficulty tiers, thresholds, and file format are our design.

## Purpose

The quiz workflow governs how written quizzes are generated, taken, graded, and adapted over time. This is the async counterpart to interactive-quiz.md — the learner completes these on their own time, then the agent grades them.

## Why This File Exists

Quizzes are the primary **evaluation** mechanism — one of the four pillars of Huntley's formula. From the X post author: the system "generates quiz (mostly open ended question) and then grade me based on my explanation & understanding, and it adjusts the next quiz based on the grade it gave me, so eventually it stops generating quizzes on stuff I'm already decent at."

This confirms three things:
1. Quizzes are open-ended (not multiple choice)
2. Grading is based on explanation and understanding (not keywords)
3. The system is adaptive — it stops asking about mastered topics

The **loopback** pattern from ghuntley.com/specs/ grounds the quiz loop directly: "Keep going until implemented. Did the LLM go on a bad path? Restart a new chat session and use the above prompt. Keep doing it until everything is implemented." Applied to learning: keep generating quizzes until everything is understood. Restart sessions. Same specs, same progress file, new quiz.

## What's Confirmed

From the X post:
- Quizzes are mostly open-ended questions ✅
- Grading is based on explanation and understanding ✅
- Next quiz adapts based on previous grades ✅
- Eventually stops generating quizzes on strong topics ✅
- Progress is tracked in md files ✅

## Quiz Generation

### Input Sources
1. `readings/*.md` — Knowledge base
2. `questions/*.md` — Interview-relevant questions
3. `progress.md` — Current learner state
4. Previous quiz files in `quiz/` — What's been asked before

### Generation Rules

<!-- YOUR INPUT NEEDED
These rules are our design. The confirmed principle is "adaptive,
open-ended, graded on understanding." Consider authoring stdlib
rules for quiz generation quality (following the stdlib pattern:
observe bad quizzes → ask agent to write a rule to prevent them).
-->

1. **Open-ended questions** — Ask the learner to explain, describe, compare, design.
2. **Weight toward weak areas** — Majority of questions target lowest-scoring topics.
3. **Include retention checks** — Keep 20-30% of questions on strong areas to prevent regression.
4. **Don't repeat exact questions** — Same topics fine, same phrasing not.
5. **Difficulty adapts** — Strong scores → harder questions. Weak scores → simpler, broken-down questions.

### Difficulty Tiers

<!-- YOUR INPUT NEEDED
The tier system is our design. You may want to let the agent decide
or author stdlib rules that govern difficulty progression.
-->

**Tier 1 — Foundational:** Define and explain basic concepts.
**Tier 2 — Applied:** How and when to use something.
**Tier 3 — Analytical:** Troubleshooting, comparison, trade-offs.
**Tier 4 — Expert:** Deep internals, edge cases, failure modes.

## Quiz File Format

Saved to `quiz/` following the timestamp rule (`rules/timestamp-generation.md`):

```markdown
# Quiz: {Topic Name}
Generated: {timestamp}
Difficulty: Tier {N}
Focus areas: {targeted weak areas from progress.md}

## Question 1
{open-ended question}

### Learner Response
{to be filled by learner}

### Grade
{to be filled by grading-workflow}

### Feedback
{to be filled by grading-workflow}

---

## Question 2
...
```

<!-- YOUR INPUT NEEDED
This file format is our design. Huntley says "let Ralph dictate
the format that works best for it." You may want to let the agent
evolve its own quiz format over time.
-->

## Adaptive Behavior (the core loop)

This is the confirmed behavior from the X post:

```
Generate quiz (weighted to weak areas)
    → Learner takes quiz
    → Agent grades (backpressure)
    → Agent updates progress.md
    → Next quiz reads updated progress.md
    → New quiz adapts
    → Loop
```

### Adaptation Rules

<!-- YOUR INPUT NEEDED
The thresholds below are our design. The X post confirms adaptive
behavior exists but does not specify thresholds. Huntley's stdlib
approach suggests: start with rough thresholds, observe where they
fail, then author rules to refine them. ~45% accuracy expected
initially — frequent steering needed (ghuntley.com/stdlib/).
-->

1. **Topic mastery** — Score ≥ 80% across 2-3 consecutive quizzes → reduce frequency, increase difficulty. Don't eliminate entirely.

2. **Persistent weakness** — Score < 50% across 2+ quizzes → break into sub-topics, drop difficulty tier, suggest specific readings, flag in progress.md.

3. **New topic introduction** — Only when foundational topics are at least Tier 2.

4. **Final review mode** — When overall progress is strong, generate comprehensive review covering everything at Tier 3-4. This is the mock interview equivalent.
