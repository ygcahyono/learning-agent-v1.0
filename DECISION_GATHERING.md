# Decision Gathering Template

A reusable template that instructs Claude Code to find open decision markers in your repo, walk through them one by one, and record decisions permanently — both in-place and in a central log. Drop this file into any repo to prevent decisions from getting lost between sessions.

---

## Quick Start

1. **Copy** this file into your project root
2. **Edit** the Configuration Block below to match your marker style and paths
3. **Add** one line to your CLAUDE.md (see Section 8)

---

## Configuration Block

Adjust these values for your project. Claude Code reads this block to know how to find and process markers.

```yaml
# DECISION_GATHERING_CONFIG
MARKER_PATTERN: "YOUR INPUT NEEDED"        # Text that identifies an open decision
DECIDED_PREFIX: "DECIDED"                   # Prefix used in replacement markers
DECISIONS_FILE: "decisions.md"              # Central log file path (relative to repo root)
SCAN_PATHS: "**/*.md"                       # Glob pattern(s) for files to scan
EXCLUDE_PATHS:                              # Files/dirs to skip
  - "DECISION_GATHERING.md"
  - "decisions.md"
  - "node_modules/"
  - ".git/"
BLOCK_COMMENT_SYNTAX: "html_comment"        # "html_comment" for <!-- -->, or "none" for plain-text markers
```

**Common configurations:**

| Project Type | MARKER_PATTERN | SCAN_PATHS | BLOCK_COMMENT_SYNTAX |
|---|---|---|---|
| Markdown-heavy (specs, docs) | `YOUR INPUT NEEDED` | `**/*.md` | `html_comment` |
| Code-heavy | `TODO:DECIDE` | `**/*.{ts,py,go,rs}` | `none` |
| Mixed | `OPEN QUESTION` | `**/*.{md,ts,py}` | `html_comment` |

---

## Supported Marker Formats

### Format A: Minimal single-line

```markdown
<!-- YOUR INPUT NEEDED: Should we use Redis or Memcached for caching? -->
```

- ID is auto-derived from `filename:line_number`
- The text after the colon is the question

### Format B: Structured multiline

```markdown
<!-- YOUR INPUT NEEDED
ID: cache-strategy
QUESTION: Should we use Redis or Memcached for caching?
CONTEXT: Current system has no caching layer. Redis offers persistence, Memcached is simpler.
OPTIONS:
- Redis (persistence, pub/sub, richer data types)
- Memcached (simpler, pure cache, slightly faster for basic key-value)
- Start without caching and add later
-->
```

- Explicit ID enables stable cross-referencing even if lines shift
- OPTIONS are presented as choices in AskUserQuestion

### Format C: Plain text (for code comments)

```python
# TODO:DECIDE Should we use sync or async HTTP client?
```

- No wrapping comment block needed
- ID auto-derived from `filename:line_number`

---

## Workflow

These are instructions for Claude Code. When a user says "gather decisions" or references this file, follow these phases in order.

### Phase 0: Pre-flight

1. Read the decisions file (DECISIONS_FILE from config). If it doesn't exist, that's fine — you'll create it in Phase 3.
2. Build a lookup of already-resolved decision IDs from the log (each `##` heading contains the ID).
3. Read this file's Configuration Block to load MARKER_PATTERN, SCAN_PATHS, EXCLUDE_PATHS, etc.

### Phase 1: Scan

1. Use Glob with SCAN_PATHS to find candidate files, excluding EXCLUDE_PATHS.
2. Use Grep with MARKER_PATTERN to find all markers across those files.
3. For each marker found:
   - **Skip** if it appears inside a code fence (between triple backticks) — those are examples, not real markers.
   - **Skip** if the marker is inside a file listed in EXCLUDE_PATHS.
   - Extract the ID: use explicit `ID:` field if present, otherwise derive as `{filename}:{line_number}`.
   - Check against the pre-flight lookup:
     - If the ID exists in the decisions log AND the source file still has the open marker → classify as **STALE** (decision was made but never applied in-place).
     - If the ID exists in the decisions log AND the source file has a `DECIDED` marker → classify as **ALREADY_RESOLVED** (skip).
     - Otherwise → classify as **OPEN**.
4. Report to the user: "Found X open decisions, Y stale (already decided but not applied), Z already resolved."

### Phase 2: Present

Walk through decisions in file order, one at a time:

1. For each **STALE** decision: show the logged decision and ask "Apply this to the source file?" — if yes, go to Phase 3 to apply it. If no, skip.
2. For each **OPEN** decision:
   - Show the question, context, and source file location.
   - If OPTIONS are provided in the marker, present them using AskUserQuestion.
   - If no OPTIONS, use AskUserQuestion with a free-text prompt.
   - If the user says "skip" or picks "Skip for now", leave the marker untouched and move to the next.
   - If the marker contains sub-questions (multiple `?` sentences or numbered items), address each sub-question before recording.

### Phase 3: Record

For each decision made:

**Step A — Replace in source file:**

Replace the entire marker block with a decided marker:

```markdown
<!-- DECIDED 2026-03-23 ID: cache-strategy DECISION: Use Redis for caching due to persistence needs and pub/sub support for future event system. -->
```

Format: `<!-- {DECIDED_PREFIX} {DATE} ID: {id} DECISION: {answer} -->`

For plain-text markers (Format C), replace the line:
```python
# DECIDED 2026-03-23 ID: http-client DECISION: Use async HTTP client for better concurrency.
```

**Step B — Append to decisions log:**

If the decisions file doesn't exist yet, create it with the header template from Section 7.

Append an entry:

```markdown
## {N} — {ID} (`{source_file}`)
**Decision:** {answer}
**Date:** {date}
**Context:** {original question text}
```

Where `{N}` is the next sequential number in the file.

### Phase 4: Summary

After all decisions are processed (or the user stops), print:

```
Decision gathering complete.
- Decided: {count} new decisions recorded
- Applied: {count} stale decisions applied in-place
- Skipped: {count} left open for later
- Remaining: {count} total open decisions still in repo
```

---

## Edge Cases

### Conflicting decisions (log vs. file disagree)
If the decisions log has an entry for an ID, but the source file has a *different* `DECIDED` marker with a different answer, flag this to the user and ask which one is correct. Update whichever is wrong.

### New markers added after a previous run
Normal operation — Phase 1 finds them as OPEN. No special handling needed.

### Multi-part decisions
If a single marker block contains multiple questions (e.g., "Should we do X? If yes, should it be Y or Z?"), treat them as sub-questions. Ask each one, then record the combined answer as a single decision entry.

### decisions.md doesn't exist yet
Create it with the header template (Section 7) on the first write.

### User abandons mid-session
Decisions already recorded in Phases 3A and 3B are permanent. Unprocessed markers remain open. No cleanup needed.

### Markers inside code fences
Any marker between ` ``` ` lines is an example/documentation. Skip it entirely. Only process markers in regular markdown content or code comments.

### Non-markdown files
For files like `.py`, `.ts`, `.go`, etc., markers use their native comment syntax (Format C). The replacement also uses native comment syntax. Only scan non-markdown files if SCAN_PATHS includes their extensions.

---

## decisions.md Header Template

When creating a new decisions file, use this header:

```markdown
# Decisions Log

Permanent record of design decisions. **Agents must check this file before asking questions to avoid re-asking resolved decisions.**

Last updated: {date}

---
```

---

## CLAUDE.md Snippet

Add this single line to your project's CLAUDE.md to activate decision gathering:

```markdown
## Decision Gathering
When asked to "gather decisions", follow the workflow in `DECISION_GATHERING.md`.
```

That's it. Claude Code will read the referenced file and follow the workflow.
