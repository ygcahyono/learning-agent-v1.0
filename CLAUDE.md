# CLAUDE.md — Operational Context for Claude Code

## What This Is

An AI-agent-powered interview prep system. I'm preparing for a **BI Manager** role with **1 week** until the interview. My biggest gap: Investor Relations, fundraising processes, and due diligence — areas I have zero experience in.

This system is built on Geoffrey Huntley's Ralph Wiggum / specs / stdlib methodology, adapted from autonomous software development to autonomous learning.

---

## How to Run a Session

**Read these files in this order, every session:**

1. `GOAL.md` — What I'm preparing for, my gaps, what success looks like
2. `progress.md` — My current state (what I know, what I don't, scores)
3. `.agents/specs/README.md` — How the learning system works
4. `.agents/stdlib/README.md` — Behavioral rules for the agent

**Then do ONE of these:**

- **Interactive quiz** — Ask me questions, I answer from memory, you correct and expand (see `.agents/specs/interactive-quiz.md`)
- **Written quiz** — Generate a quiz file in `quiz/`, I fill it in, you grade it (see `.agents/specs/quiz-workflow.md`)
- **Readings generation** — Create study material in `readings/` from my source books and JD (see source material below)

**After every session:**
- Grade my responses using `.agents/specs/grading-workflow.md`
- Update `progress.md` with new scores and findings
- Identify what to focus on next session

---

## Source Material (in readings/)

- **Venture Deals book** — chapters will be added as PDF or markdown files
- **Job description** — will be placed as `readings/jd.md`
- Any additional IR/fundraising material I add over the week

The agent should generate quizzes and questions **from this material**, weighted toward my weak areas (which is almost everything IR-related right now).

---

## The Formula

```
/specs + /stdlib + loopback + evaluation = good outcomes
```

- **Specs** (`.agents/specs/`) — How the learning system behaves
- **Stdlib** (`.agents/stdlib/`) — Rules that steer agent behavior (explanation format, handling uncertainty, verification, topic decomposition)
- **Loopback** — Keep quizzing until understood. Each session reads progress, acts on gaps, updates progress. Restart context, same approach, repeatedly.
- **Evaluation** — Backpressure. Grade responses honestly. No "good job" for shallow answers. Push for depth. Surface-level = reject. "It's best practice" without WHY = reject.

---

## Key Principles

- **Honesty over completeness** — If I don't know something, push me to figure it out before revealing the answer
- **No placeholders** — Don't accept vague answers. "Due diligence is when investors check things" is not good enough.
- **Capture the why** — Every correction should explain WHY it matters, not just what the right answer is
- **Urgent timeline** — 1 week. Don't quiz me on BI fundamentals I already know. Focus entirely on IR/fundraising/due diligence and how BI connects to them.
- **Interview-ready framing** — Questions should sound like what an interviewer would ask. Answers should be practiced in a way I can actually say them in an interview.

---

## File Structure

```
GOAL.md                    # Role, gaps, success criteria, priority topics
CLAUDE.md                  # This file — operational instructions for the agent
PROMPT.md                  # Session launcher prompt
progress.md                # Persistent learner state (updated every session)
readings/                  # Source material (book chapters, JD, articles)
questions/                 # Common interview questions (agent-generated)
quiz/                      # Timestamped quiz files
sessions/                  # Session transcripts
practice/                  # Practice exercises (if applicable)
.agents/specs/             # Learning system specifications
.agents/stdlib/            # Behavioral rules for the agent
.agents/rules/             # File naming and formatting rules
references/                # Methodology source material (Huntley PDFs)
```

---

## Methodology Background

Built on Huntley's Ralph Wiggum technique. For methodology details, see `references/` PDFs. The key adaptation: "keep going until implemented" becomes "keep quizzing until understood." The agent loop runs one session at a time, the human is the orchestrator, and `progress.md` is the persistent memory between sessions.
