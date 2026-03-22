# Rule: Timestamp Generation

> **Source fidelity: 90%** — Filename pattern directly visible in the original X post screenshot. Specific formatting rules inferred from those filenames.

## Purpose

All generated quiz files and session artifacts must include ISO 8601 timestamps in their filenames. This ensures chronological ordering, enables progress tracking over time, and prevents filename collisions.

## Format

```
{topic}-{ISO8601_timestamp}.md
```

### Timestamp format

```
YYYY-MM-DDTHH-MM-SS+TZ-TZ
```

Example: `2026-03-07T12-33-58+07-00`

Note: Colons in the time portion are replaced with hyphens for filesystem compatibility.

## Examples (from original screenshot)

```
linux-fundamentals-2026-03-07T12-33-58+07-00.md
linux-fundamentals-2026-03-07T15-57-15+07-00.md
linux-fundamentals-final-review-2026-03-07T23-38-11+07-00.md
linux-fundamentals-quiz3-2026-03-07T18-30-25+07-00.md
mock-interview-2026-03-09T20-50-37+07-00.md
round2-prep-2026-03-10T20-37-22+07-00.md
storage-fundamentals-2026-03-08T20-53-26+07-00.md
```

## Rules

1. **Always generate a timestamp** — Every quiz, review, or graded artifact must have a timestamp.
2. **Use the learner's local timezone** — Do not default to UTC.
3. **Topic slug first, then timestamp** — Files sort by topic, then chronologically.
4. **Hyphens as separators** — Replace colons in HH:MM:SS with hyphens.
5. **Include timezone offset** — Always include UTC offset (e.g., `+07-00`, `+00-00`).

## Rationale

This creates an audit trail of the learning process — the agent can reconstruct the full timeline by reading the `quiz/` directory. This feeds directly into progress tracking and adaptive quiz generation.

<!-- YOUR INPUT NEEDED
The 10% gap:
- I inferred the hyphen-for-colon convention from the screenshot filenames
- The original author may have additional rules for special quiz types
  (e.g., "final-review", "quiz3", "round2-prep" naming conventions)
- If you have preferences for how topic slugs are formatted, specify here
-->
