# Spec: Technical Verification

> **Source fidelity: 50%** — Directly inspired by Huntley's "if you find inconsistencies in the specs then use the oracle and update the specs" and "don't assume not implemented." The principle of verifying before acting is core Huntley. The specific verification methods and process are my extrapolation.

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
Huntley refers to "the oracle" but doesn't specify verification
methods in detail (that content may be in the paywalled posts).

The methods below are my design:

1. Web research — using search tools (EXA, web search, etc.)
2. Official documentation — man pages, vendor docs, RFCs
3. Cross-referencing — when sources disagree, prefer official > blog

You should decide:
- What tools your agent has access to for verification
  (EXA, web search, MCP servers, etc.)
- How much verification overhead is acceptable per session
- Whether to verify everything or only flag uncertain claims
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

- How aggressive should verification be? Verify everything
  (slow, thorough) or only verify when uncertain (faster)?

- Should the agent document its verification sources in the
  readings? (Creates audit trail but adds noise)

- For topics you already know well, do you want the agent to
  still verify, or trust your domain knowledge as an input?

- The original X post author used EXA for web research.
  What tools will your agent have access to?
-->
