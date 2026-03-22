# Spec: Quiz Workflow

> **Source fidelity: 45%** — The X post confirms: quizzes are mostly open-ended, they get graded, and the system adapts based on grades ("it adjusts the next quiz based on the grade it gave me, so eventually it stops generating quizzes on stuff I'm already decent at"). The adaptive behavior is confirmed. The specific generation rules, difficulty tiers, thresholds, and file format are my design.

## Purpose

The quiz workflow governs how written quizzes are generated, taken, graded, and adapted over time. This is the async counterpart to interactive-quiz.md — the learner completes these on their own time, then the agent grades them.

## Why This File Exists

Quizzes are the primary backpressure mechanism. From the X post author: the system "generates quiz (mostly open ended question) and then grade me based on my explanation & understanding, and it adjusts the next quiz based on the grade it gave me, so eventually it stops generating quizzes on stuff I'm already decent at."

This confirms three things:
1. Quizzes are open-ended (not multiple choice)
2. Grading is based on explanation and understanding (not keywords)
3. The system is adaptive — it stops asking about mastered topics

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
These rules are my design. The confirmed principle is "adaptive,
open-ended, graded on understanding." The specific rules below
are my implementation of that principle. Adjust as needed.
-->

1. **Open-ended questions** — Ask the learner to explain, describe, compare, design.
2. **Weight toward weak areas** — Majority of questions target lowest-scoring topics.
3. **Include retention checks** — Keep 20-30% of questions on strong areas to prevent regression.
4. **Don't repeat exact questions** — Same topics fine, same phrasing not.
5. **Difficulty adapts** — Strong scores → harder questions. Weak scores → simpler, broken-down questions.

### Difficulty Tiers

<!-- YOUR INPUT NEEDED
The tier system is my design. The original author may have used
a different difficulty model or let the agent decide.

Tier 1: "What is X?"
Tier 2: "How would you configure X for Y?"
Tier 3: "Why choose X over Y? What are the trade-offs?"
Tier 4: "What happens when X fails during Y? How would you recover?"
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
This file format is my design. You may want:
- Different sections
- A separate answer file (so quizzes can be retaken)
- Metadata fields (time spent, difficulty rating by learner)
- The agent to evolve its own format
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
The thresholds below are my invention. The X post confirms
adaptive behavior exists but does not specify thresholds.

Key numbers to tune:
- Mastery threshold: I used 80% across 2-3 quizzes
- Weakness escalation: I used <50% across 2+ quizzes
- These should be adjusted based on your experience

You may also want to:
- Use different thresholds per topic
- Let the agent decide when a topic is mastered
- Add a "time since last studied" decay factor
-->

1. **Topic mastery** — Score ≥ 80% across 2-3 consecutive quizzes → reduce frequency, increase difficulty. Don't eliminate entirely.

2. **Persistent weakness** — Score < 50% across 2+ quizzes → break into sub-topics, drop difficulty tier, suggest specific readings, flag in progress.md.

3. **New topic introduction** — Only when foundational topics are at least Tier 2.

4. **Final review mode** — When overall progress is strong, generate comprehensive review covering everything at Tier 3-4. This is the mock interview equivalent.
