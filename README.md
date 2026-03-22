# Learning Agent v1.0

An AI-agent-powered self-study system built on principles from Geoffrey Huntley's "Ralph Wiggum" technique and specs/stdlib methodology, adapted from autonomous software development to autonomous learning.

## Origin

This project is inspired by an [X post](https://x.com/) where someone described using AI agents with a `.agents/` folder structure (rules + specs) to cram for a technical interview over a weekend — and passed. They credited Geoffrey Huntley's blog posts for the methodology.

The original `.agents/` folder was not open-sourced. This repo is a reconstruction based on publicly available Huntley content.

---

## Source Material & Access Status

| Source | URL | Access | Notes |
|--------|-----|--------|-------|
| Ralph Wiggum post | https://ghuntley.com/ralph/ | ✅ Full access | Core methodology, actual PROMPT.md templates, loop mechanics |
| Ralph Playbook repo | https://github.com/ghuntley/how-to-ralph-wiggum | ✅ Full access | Comprehensive playbook with workflow, guidelines, file templates, enhancements |
| Everything is a Ralph Loop | https://ghuntley.com/loop/ | ✅ Full access | Monolithic agents, orchestrator pattern |
| How to Build a Coding Agent | https://ghuntley.com/agent/ | ✅ Full access | Workshop transcript, context windows, model selection |
| Don't Waste Your Backpressure (Moss) | https://banay.me/dont-waste-your-backpressure/ | ⚠️ Search snippets only | Referenced by Huntley, core backpressure philosophy |
| Specs methodology | https://ghuntley.com/specs/ | ❌ Paywalled | How to write specs from JTBD conversations |
| Stdlib methodology | https://ghuntley.com/stdlib/ | ❌ Paywalled | How to write standard libraries to steer agent behavior |
| Don't Waste Your Backpressure (Huntley) | https://ghuntley.com/pressure/ | ❌ Paywalled | Huntley's own backpressure elaboration |

**Bottom line:** The Ralph post and the Playbook repo gave me the structural principles (loop, specs, backpressure, progress state, one task per loop, no placeholders). The paywalled specs/stdlib posts mean I could NOT see how Huntley specifically structures individual spec files or stdlib entries. That gap is reflected in the fidelity percentages below.

---

## File Map & Source Fidelity

Every file in this repo has a **source fidelity score** — what percentage is directly grounded in Huntley's accessible content vs. what I inferred or extrapolated.

### `.agents/rules/`

| File | Purpose | Fidelity | Explanation |
|------|---------|----------|-------------|
| `timestamp-generation.md` | How to name quiz files with timestamps | **90%** | The original X post screenshot clearly shows timestamped filenames. The ISO 8601 format and naming convention is directly visible. The 10% gap: I inferred the specific rules (timezone handling, hyphen conventions) from the file names in the screenshot. |

### `.agents/specs/`

| File | Purpose | Fidelity | Explanation |
|------|---------|----------|-------------|
| `README.md` | Overview of the system, how files relate, how to use it | **70%** | The mapping from Ralph concepts to learning concepts is solid (specs→workflow specs, IMPLEMENTATION_PLAN→progress, backpressure→grading). The usage instructions and "how it works" loop are my interpretation of how you'd apply Ralph to learning. The 30% gap: I couldn't see Huntley's actual specs README to know how he introduces a specs folder. |
| `interactive-quiz.md` | Socratic Q&A tutoring — agent asks, learner answers, agent corrects and expands | **40%** | The X post author explicitly described preferring interactive learning where "the agent asks a question and lets me try to answer with my current existing knowledge and then expand & correct it, kinda like a personal tutor." That's the WHAT. The HOW (specific workflow steps, question types, anti-patterns, session structure) is my extrapolation applying Huntley's principles (one task per loop, no placeholders, backpressure). I could not access the paywalled specs post to see how Huntley structures a spec file internally. |
| `quiz-workflow.md` | End-to-end quiz lifecycle — generation, administration, adaptive difficulty | **45%** | The X post confirms: quizzes exist, they're mostly open-ended, they get graded, and the next quiz adapts based on grades ("it adjusts the next quiz based on the grade it gave me, so eventually it stops generating quizzes on stuff I'm already decent at"). That's the WHAT. The specific generation rules, difficulty tiers, adaptation thresholds (80%, 50%), and file format are my design applying Huntley's principles. |
| `grading-workflow.md` | How to grade quiz responses — the backpressure mechanism | **55%** | Huntley's backpressure philosophy is well-documented in accessible posts: "capture the why," "no placeholder implementations," tests must exist and pass, backpressure rejects invalid output. I mapped these directly to grading (reject surface-level answers, capture why answers are wrong, push for depth). The 1-5 scoring scale and specific grading process steps are my design. |
| `progress-tracking.md` | Persistent state file tracking learner competency across sessions | **60%** | Directly mapped from Huntley's `IMPLEMENTATION_PLAN.md` and `fix_plan.md` — the file that persists between loops, drives what happens next, is disposable/regeneratable, and should be kept current. The Ralph Playbook repo explicitly documents this pattern. The specific table structure, status categories, session history format, and cleanup rules are my design. |
| `practice-labs.md` | Hands-on lab exercise generation with verification | **35%** | The X post confirms practice labs exist ("it told me to setup LVM stuff on a GCP VM"). Huntley's "loop back" principle (loop the agent back on itself for evaluation) maps to verification steps. But the specific lab types, generation rules, environment options, and file format are largely my design. |
| `technical-verification.md` | Ensuring learning materials are technically accurate | **50%** | Directly inspired by Huntley's "if you find inconsistencies in the specs then use the oracle and update the specs" and "don't assume not implemented." The principle of verifying before teaching is solid Huntley. The specific verification methods, handling uncertainty, and integration points are my extrapolation. |

### Other folders

| Folder | Purpose | Contents |
|--------|---------|----------|
| `readings/` | Agent-generated study materials per topic | Empty — populated per topic |
| `questions/` | Commonly asked questions per topic (interview-relevant) | Empty — populated per topic |
| `practice/` | Generated hands-on lab exercises | Empty — populated by agent |
| `quiz/` | Timestamped quiz files (generated, taken, graded) | Empty — populated by agent |
| `progress.md` | Persistent learner state (the heart of the loop) | Empty — populated by agent |

---

## What Each File Does (Bottom-Up)

Read these in this order to understand the system from foundations up:

### Layer 1: The Rule
**`rules/timestamp-generation.md`** — The simplest file. Just says "name your quiz files like this: `{topic}-{timestamp}.md`". Exists so every generated artifact has a consistent, sortable name. Without this, the quiz folder becomes chaos.

### Layer 2: The State
**`specs/progress-tracking.md`** — Defines how `progress.md` works. This is the single most important file because it's the memory between sessions. Every other spec reads from and writes to progress. If you change nothing else, understand this one.

### Layer 3: The Backpressure
**`specs/grading-workflow.md`** — Defines how the agent evaluates your answers. This is what prevents the system from being a passive reading experience. Without grading, there's no feedback signal and no adaptation. The grading output feeds into progress tracking.

### Layer 4: The Learning Mechanisms
**`specs/interactive-quiz.md`** — The real-time Socratic method. Agent asks, you answer, agent corrects.
**`specs/quiz-workflow.md`** — The async written quiz. Agent generates, you write answers, agent grades.
**`specs/practice-labs.md`** — The hands-on component. Agent generates tasks, you execute, results verify.

These three are different modes of the same loop. They all read from progress, they all feed grades back into progress.

### Layer 5: Quality Control
**`specs/technical-verification.md`** — Ensures the agent isn't teaching you wrong things. Meta-level concern that applies to all other specs.

### Layer 6: The Entry Point
**`specs/README.md`** — Ties everything together. Explains how the files relate, how to use the system, and maps Huntley concepts to learning concepts.

---

## What's Missing / Needs Your Input

These are areas where the paywalled content means I was working with incomplete information. Marked with `<!-- YOUR INPUT NEEDED -->` inside the relevant files.

1. **Internal spec structure** — I couldn't see how Huntley formats a spec file internally (ghuntley.com/specs is paywalled). The current format is my best guess. If you get access to the specs post, you may want to restructure.

2. **Stdlib patterns** — Huntley's stdlib concept (ghuntley.com/stdlib, paywalled) is about creating reusable "standard library" patterns that steer agent behavior. I don't have a `stdlib/` equivalent in this repo. You might want one for things like "how to format explanations" or "how to structure reading materials."

3. **PROMPT.md and loop.sh** — The actual launcher files (what you'd pipe into Claude Code) are NOT included yet. The Ralph Playbook repo has clear templates for these. I left them out because they depend on your specific CLI setup.

4. **Adaptation thresholds** — The specific numbers I used (80% for mastery, 50% for weakness escalation, 2-3 consecutive quizzes) are my invention. You should tune these based on experience.

---

## How to Use

```bash
# Clone this repo
git clone https://github.com/ygcahyono/learning-agent-v1.0.git
cd learning-agent-v1.0

# When you want to learn a topic (e.g., Logistic Regression):
# 1. Ask the agent to generate readings/ and questions/
# 2. Start interactive sessions or generate quizzes
# 3. Agent grades and updates progress.md
# 4. Next session adapts based on progress
# 5. Loop until mastery
```

---

## References

- Geoffrey Huntley's blog: https://ghuntley.com/
- Ralph Wiggum post: https://ghuntley.com/ralph/
- Ralph Playbook: https://github.com/ghuntley/how-to-ralph-wiggum
- Backpressure (Moss): https://banay.me/dont-waste-your-backpressure/
- Latent Patterns: https://latentpatterns.com/
