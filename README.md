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
| Don't Waste Your Backpressure (Moss) | https://banay.me/dont-waste-your-backpressure/ | ✅ PDF in references/ | Layered backpressure model: each layer (compiler, type system, MCP servers, proof assistants) reduces human feedback burden and increases task complexity ceiling. Loop agents until inconsistencies are stamped out. "Are you wasting your back pressure?" |
| Specs methodology | https://ghuntley.com/specs/ | ✅ PDF in references/ | Specs = functional specifications, one per domain topic. SPECS.md overview. Loopback prompt. The formula. |
| Stdlib methodology | https://ghuntley.com/stdlib/ | ✅ PDF in references/ | Stdlib = library of prompting rules composed like unix pipes. Rule structure, authoring workflow, exponential growth. |
| Don't Waste Your Backpressure (Huntley) | https://ghuntley.com/pressure/ | ✅ PDF in references/ | Endorses Moss's banay.me article. Adds tuning metaphor: backpressure needs calibration — too little and hallucinations pass, too much and the wheel stops. Pre-commit hooks as favorite mechanism. "If you aren't capturing your back-pressure then you are failing as a software engineer." |

**Bottom line:** The Ralph post, the Playbook repo, and now the specs, stdlib, and backpressure PDFs give us the full picture. All previously paywalled sources are now accessible. The remaining gap is the specific adaptation of these coding-focused patterns to learning, which is our interpretation.

### The Formula (from ghuntley.com/specs/)

```
/specs + /stdlib + loopback + evaluation = good outcomes
```

- **`/specs`** — Functional specifications, one per domain topic (`specs/`)
- **`/stdlib`** — Behavioral rules that steer agent behavior (`stdlib/`)
- **`loopback`** — "Keep going until implemented." Restart context, same prompt, repeatedly.
- **`evaluation`** — Backpressure. Tests/build in coding; grading/quizzes in learning.

The key loopback prompt from the specs post: "Study @SPECS.md for functional specifications. Study @.cursor for technical requirements. Implement what is not implemented. Create tests. Run build and verify."

---

## File Map & Source Fidelity

Every file in this repo has a **source fidelity score** — what percentage is directly grounded in Huntley's accessible content vs. what I inferred or extrapolated.

### `.agents/rules/`

| File | Purpose | Fidelity | Explanation |
|------|---------|----------|-------------|
| `timestamp-generation.md` | How to name quiz files with timestamps | **90%** | The original X post screenshot clearly shows timestamped filenames. The ISO 8601 format and naming convention is directly visible. The 10% gap: specific rules (timezone handling, hyphen conventions) inferred from the file names in the screenshot. |

### `.agents/specs/`

| File | Purpose | Fidelity | Explanation |
|------|---------|----------|-------------|
| `README.md` | Overview of the system, how files relate, how to use it | **85%** | The mapping from Ralph concepts to learning concepts is now well-grounded. The specs post confirms SPECS.md overview format and the formula. The stdlib post confirms behavioral rules concept. The 15% gap: specific application to learning (vs. coding) is our interpretation. |
| `interactive-quiz.md` | Socratic Q&A tutoring — agent asks, learner answers, agent corrects and expands | **55%** | The X post confirms WHAT this does. The loopback pattern from the specs post ("keep going until implemented" = keep quizzing until understood) now directly grounds the iterative approach. The HOW (specific workflow steps, question types) is our extrapolation. |
| `quiz-workflow.md` | End-to-end quiz lifecycle — generation, administration, adaptive difficulty | **60%** | The X post confirms adaptive quiz behavior. The loopback and evaluation pillars from the formula now directly ground the quiz loop. The specific generation rules, difficulty tiers, and thresholds are our design. |
| `grading-workflow.md` | How to grade quiz responses — the backpressure mechanism | **70%** | Backpressure philosophy is well-documented. Grading is now grounded as the "evaluation" component of the formula. The tuning metaphor from ghuntley.com/pressure/ adds calibration nuance. The 1-5 scoring scale and specific grading process are our design. |
| `progress-tracking.md` | Persistent state file tracking learner competency across sessions | **70%** | Directly mapped from IMPLEMENTATION_PLAN.md. The specs post confirms the SPECS.md overview pattern (table linking to details) which maps to the progress overview. The specific table structure and cleanup rules are our design. |
| `practice-labs.md` | Hands-on lab exercise generation with verification | **40%** | The X post confirms practice labs exist. The loopback pattern applies. The specific lab types, generation rules, and file format are largely our design. |
| `technical-verification.md` | Ensuring learning materials are technically accurate | **65%** | The self-improvement loop from the stdlib post ("Look at rules. What's missing?") now fully grounds the meta-verification pattern. The specific verification methods are our extrapolation. |

### `.agents/stdlib/`

| File | Purpose | Fidelity | Explanation |
|------|---------|----------|-------------|
| `README.md` | Stdlib overview, rule structure, authoring guide | **80%** | Rule structure (description, globs, filters, actions, examples, metadata) directly sourced from stdlib post. Authoring workflow ("observe bad behavior → author rule") directly sourced. |
| `explanation-format.md` | How to format explanations for the learner | **70%** | Follows the stdlib rule structure exactly. The specific explanation guidelines are our design for learning. |
| `handle-uncertainty.md` | How to handle "I don't know" responses | **70%** | Follows the stdlib rule structure. The guided discovery approach is our adaptation. |
| `verify-before-teaching.md` | Always verify claims before teaching | **75%** | Directly from "use the oracle and update the specs." Rule structure from stdlib post. |
| `break-down-weak-topics.md` | Break topics into sub-topics when score is low | **70%** | Follows from "don't assume not implemented" applied to learning. Rule structure from stdlib post. |

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

### Layer 5: The Behavioral Rules
**`stdlib/*.md`** — Rules that steer agent behavior. From the stdlib post: "instead of 'implement XYZ' you should be thinking of building out a stdlib of thousands of prompting rules." These rules are composed like unix pipes — each one constrains one aspect of behavior (explanation format, uncertainty handling, verification, topic decomposition). The stdlib grows over time as you observe and correct agent behavior.

### Layer 6: Quality Control
**`specs/technical-verification.md`** — Ensures the agent isn't teaching you wrong things. Meta-level concern that applies to all other specs. Now includes the self-improvement loop from the stdlib post: "Look at the rules. What is missing? What does not follow best practice."

### Layer 7: The Entry Point
**`specs/README.md`** — Ties everything together. The SPECS.md overview (from ghuntley.com/specs/: "Create a SPECS.md in the root of the directory which is an overview document that contains a table that links to all the specs"). Maps Huntley concepts to learning concepts. Documents the formula.

---

## What's Missing / Needs Your Input

Marked with `<!-- YOUR INPUT NEEDED -->` inside the relevant files.

1. ~~**Internal spec structure**~~ — **Resolved.** The specs post confirms: one spec per domain topic, SPECS.md overview with table linking to all specs. Our current format is close to Huntley's pattern.

2. ~~**Stdlib patterns**~~ — **Resolved.** The stdlib post is now fully accessible. We've created `.agents/stdlib/` with four example rules following Huntley's exact rule structure (description, globs, filters, actions, examples, metadata). Grow this library over time by observing agent behavior and authoring new rules.

3. **PROMPT.md and loop.sh** — The actual launcher files are NOT included yet. The Ralph Playbook repo has templates. The specs post provides the key loopback prompt: "Study @SPECS.md for functional specifications. Study @.cursor for technical requirements. Implement what is not implemented. Create tests. Run build and verify." Adapt this for learning.

4. **Adaptation thresholds** — The specific numbers (80% mastery, 50% weakness escalation) are our design. The stdlib approach says: start with rough values, observe failures, author rules to refine. ~45% accuracy expected initially — frequent steering needed.

5. ~~**Huntley's backpressure post**~~ — **Resolved.** ghuntley.com/pressure/ is now accessible as a PDF. The post mostly endorses Moss's banay.me article. The genuinely new insight is the **tuning metaphor**: backpressure needs calibration — too little resistance and hallucinations pass through, too much and progress stalls. Applied to learning: quiz difficulty and grading strictness must be tuned, not just turned on.

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
