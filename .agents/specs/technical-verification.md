# Spec: Technical Verification

> **Source fidelity: 65%** — Directly inspired by Huntley's "if you find inconsistencies in the specs then use the oracle and update the specs" and "don't assume not implemented." The stdlib post adds the self-improvement loop: "Look at the rules. What is missing? What does not follow best practice." Applied to learning: look at the learning materials — what's missing, what's inaccurate, what doesn't follow best practice? The specific verification methods are our extrapolation.

## Purpose

Ensures that the learning materials themselves are accurate. Before the agent can teach, it must verify that what it's teaching is correct. This is a meta-level quality control spec.

## Why This File Exists

From Huntley (ghuntley.com/ralph/):
- "If you find inconsistencies in the specs/* then use the oracle and then update the specs."
  → "The oracle" = external research (web search, official docs)
  → Applied to learning: verify readings against authoritative sources

- "Before making changes search codebase (don't assume not implemented)"
  → Applied to learning: before teaching, search and verify the claim is accurate

- A lesson from building CURSED: "my specification for the lexer defined a keyword twice for two opposing scenarios, which resulted in a lot of time wasted. Ralph was doing stupid shit, and I guess it's easy to blame the tools instead of the operator."
  → Applied to learning: bad specs (inaccurate readings) produce bad learning

## The Self-Improvement Loop (from ghuntley.com/stdlib/)

The stdlib post reveals a critical self-improvement pattern:

> "Look at the rules in @.cursor. What is missing? What does not follow best practice."

This is a meta-verification loop: the agent examines its own rules and identifies gaps. Applied to learning:

1. **Look at the learning materials** — What's missing? What's inaccurate?
2. **Look at the grading** — Are we grading on the right things?
3. **Look at the rules** — Are our stdlib rules catching the right problems?
4. **Author new rules** — When you observe bad behavior, ask the agent to write a rule to prevent it.

This creates exponential improvement: "Improvements come slowly in the beginning, but your gains increase rapidly over time." (ghuntley.com/stdlib/)

## When to Verify

### During Reading Generation
- Cross-reference technical claims against official documentation
- Verify command syntax against man pages or --help output
- Confirm version-specific behavior
- Check that examples are functional

### During Quiz Generation
- Verify expected answers are correct
- Ensure questions aren't based on outdated information

### During Grading
- Don't grade based on assumptions — verify the correct answer first
- The learner might be right and the agent's knowledge might be wrong

### During Lab Generation
- Verify commands work on specified OS/version
- Check configuration paths are correct
- Ensure package names are current

## Verification Methods

<!-- YOUR INPUT NEEDED
The verification methods below are our design. Huntley refers to
"the oracle" for verification. You should decide:
- What tools your agent has access to (EXA, web search, MCP servers)
- How much verification overhead is acceptable per session
- Whether to verify everything or only flag uncertain claims
- Whether to author stdlib rules for verification behavior
-->

### 1. Web Research (Primary)
Use search tools to verify against:
- Official documentation (man pages, vendor docs)
- RFC documents for protocol-level claims
- Reputable technical sources

### 2. Command Verification
For any command in materials:
- Verify the command exists and flags are correct
- Verify expected output matches reality
- Note version-specific differences

### 3. Cross-Reference
When sources disagree:
- Prefer official documentation over blog posts
- Prefer newer sources over older ones
- Note the disagreement rather than silently picking one

## Handling Uncertainty

If the agent cannot verify a claim:
1. Mark it explicitly as unverified
2. Do not present it as fact
3. Flag for the learner to research themselves

<!-- YOUR INPUT NEEDED
Open questions:

- How aggressive should verification be? Huntley's stdlib
  approach: start with light verification, observe failures,
  author rules to catch them. ~45% accuracy expected initially.

- Should the agent document its verification sources?

- The original X post author used EXA for web research.
  What tools will your agent have access to?
-->
