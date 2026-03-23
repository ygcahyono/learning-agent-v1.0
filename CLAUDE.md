# CLAUDE.md — Project Context for Claude Code

## What This Project Is

An AI-agent-powered self-study system built on principles from Geoffrey Huntley's "Ralph Wiggum" technique, specs methodology, and stdlib methodology. Adapted from autonomous software development to autonomous learning.

Inspired by an X post where someone used `.agents/` folder with rules + specs to cram for a technical interview over a weekend using AI agents — and passed.

GitHub repo: https://github.com/ygcahyono/learning-agent-v1.0.git

---

## Source Material

### Fully accessible sources (already digested)

| Source | Key Takeaways |
|--------|---------------|
| **ghuntley.com/ralph/** | Core Ralph technique: bash loop, one task per loop, specs drive behavior, backpressure (tests/build) rejects invalid output, IMPLEMENTATION_PLAN.md as persistent state, capture the why, no placeholder implementations, plan is disposable, subagents extend context, deterministic context loading every loop |
| **github.com/ghuntley/how-to-ralph-wiggum** | Full Ralph Playbook: 3 phases (define requirements → planning loop → building loop), PROMPT templates for plan and build modes, AGENTS.md as operational guide, guidelines (context is precious, simplicity wins, markdown over JSON, tune like a guitar), enhanced loop.sh script, file structure |
| **ghuntley.com/loop/** | Everything is a Ralph loop. Monolithic agents over multi-agent. Single process, one task per loop. Orchestrator pattern. |
| **ghuntley.com/agent/** | Workshop transcript. Context windows (~176k usable from 200k advertised). MCP allocation. Model selection (Opus for reasoning, Sonnet for efficiency). |
| **banay.me backpressure article** | Backpressure = automated feedback (tests, linters, type systems) that lets agents self-correct without human intervention. Without it, you waste human time on syntax errors instead of architecture decisions. |
| **ghuntley.com/pressure/** | Endorses Moss's banay.me article. Adds tuning metaphor: backpressure needs calibration — too little and hallucinations pass, too much and the wheel stops. Pre-commit hooks as favorite mechanism. "If you aren't capturing your back-pressure then you are failing as a software engineer." |

### Previously paywalled sources (NOW AVAILABLE as PDFs in references/)

**IMPORTANT: These two PDFs contain critical information that was missing when the spec files were first written. Claude Code must read these PDFs to update the files.**

| File | Source | Key Takeaways |
|------|--------|---------------|
| `references/ghuntley-stdlib-2026-03-23-00_00_30.pdf` | ghuntley.com/stdlib/ | **Stdlib = a library of prompting RULES (not code).** Thousands of prompting rules composed like unix pipes. Rules have structure: description, globs, filters (type: content/event/file_extension/intent/file_name), actions (reject/suggest/execute), examples, metadata (priority, version). The LLM writes the rules itself — you observe bad behavior, ask LLM to author a rule. ~45% accuracy expected, so frequent steering needed. Loop back: "Look at rules. What's missing? What doesn't follow best practice?" Exponential growth curve as stdlib grows. Key quote: "building out a stdlib (standard library) of thousands of prompting rules and then composing them together like unix pipes." |
| `references/ghuntley-specs-2026-03-22-23_59_14.pdf` | ghuntley.com/specs/ | **Specs = functional specifications, one per domain topic.** Stored in `specs/` with `SPECS.md` overview. Specification domains map to parts of the application. How to create: long conversation about requirements → ask LLM to write specs, one per domain, with overview. Key loopback prompt: "Study @SPECS.md for functional specifications. Study @.cursor for technical requirements. Implement what is not implemented. Create tests. Run build and verify." Keep going until implemented — restart context, same prompt, repeatedly. Formula: `/specs + /stdlib + loopback + evaluation = good outcomes`. Specs are the heart — implementation details matter less. |

---

## Current State of Files

Every file has a **source fidelity score** — what percentage is grounded in actual Huntley content vs. inferred/extrapolated. Files contain `<!-- YOUR INPUT NEEDED -->` blocks marking areas where the author's input is needed.

| File | Current Fidelity | Target After Update | What Needs Changing |
|------|-----------------|--------------------|--------------------|
| `README.md` | — | — | Update source access table (both PDFs now ✅). Add stdlib concept explanation. Add the `/specs + /stdlib + loopback + evaluation` formula. Adjust all fidelity scores to new values. |
| `.agents/specs/README.md` | 70% | 85% | Major update. Now have actual specs + stdlib methodology. Add stdlib concept and how it maps to learning. Update concept mapping table with stdlib row. Explain the formula applied to learning. |
| `.agents/rules/timestamp-generation.md` | 90% | 90% | No change needed from PDFs. |
| `.agents/specs/grading-workflow.md` | 55% | 70% | Backpressure is now better grounded with the formula and the tuning metaphor from ghuntley.com/pressure/. Update sourced sections. Reduce `<!-- YOUR INPUT NEEDED -->` blocks where we now have answers. |
| `.agents/specs/interactive-quiz.md` | 40% | 55% | Loopback pattern ("keep going until implemented" = "keep quizzing until understood") now directly sourced. Update accordingly. |
| `.agents/specs/quiz-workflow.md` | 45% | 60% | Same loopback grounding. Evaluation as part of the formula now sourced. |
| `.agents/specs/progress-tracking.md` | 60% | 70% | SPECS.md overview concept maps to having a progress overview. |
| `.agents/specs/practice-labs.md` | 35% | 40% | Minimal new info for labs. Small update only. |
| `.agents/specs/technical-verification.md` | 50% | 65% | Self-improvement loop ("look at rules, what's missing, what doesn't follow best practice") now fully sourced. |

### New file to consider

**`stdlib/` folder concept** — Huntley's stdlib is a library of behavioral rules that steer the agent. For learning, this could be rules like:
- "How to format explanations"
- "How to handle 'I don't know' responses"  
- "Always verify claims before teaching"
- "When learner scores below X, break topic into sub-topics"

This was flagged as a gap in the original specs README. Now we have the actual pattern from the stdlib PDF. Consider whether to add a `stdlib/` or `.agents/stdlib/` folder with example learning rules following Huntley's rule structure.

---

## Task for Claude Code

### Phase 1: Read source material
1. Read this CLAUDE.md fully
2. Read `references/ghuntley-specs-2026-03-22-23_59_14.pdf`
3. Read `references/ghuntley-stdlib-2026-03-23-00_00_30.pdf`
4. Read all existing files in `.agents/specs/` and `.agents/rules/`

### Phase 2: Update existing files
For each file listed in the table above:
1. Read the current content
2. Incorporate new information from the PDFs
3. Update the fidelity score at the top of each file
4. Remove or reduce `<!-- YOUR INPUT NEEDED -->` blocks where we now have answers from the PDFs
5. Keep `<!-- YOUR INPUT NEEDED -->` blocks where human input is still genuinely needed
6. Do NOT fabricate — if something is still unknown after reading the PDFs, leave it as open space

### Phase 3: Consider new additions
1. Evaluate whether a `stdlib/` concept should be added (rules that steer agent learning behavior, following Huntley's rule structure from the stdlib PDF)
2. If yes, create the folder and example files
3. Update README.md and specs/README.md to reference it

### Phase 4: Update README.md
1. Update the source access table (both paywalled sources now ✅)
2. Update all fidelity scores
3. Add the `/specs + /stdlib + loopback + evaluation` formula explanation
4. Add stdlib concept to the "What's Missing" section (or remove it if addressed)

### Principles
- **Honesty over completeness** — don't fill gaps with guesses
- **One task at a time** — update files one by one, verify each before moving on
- **Capture the why** — when updating, explain why the change was made
- **Preserve open spaces** — human input areas should remain where genuinely needed
