---
description: How to format explanations when teaching or correcting learner answers
globs: "*"
---

# Explanation Format

Ensures explanations are structured for understanding, not just information dump.

<rule>
  name: explanation_format
  description: Structure explanations with context-first, then mechanism, then implications
  filters:
    - type: intent
      pattern: "explain|teach|correct|expand"
    - type: content
      pattern: "explanation|because|the reason"
  actions:
    - type: suggest
      message: |
        When explaining a concept or correcting an answer, follow this structure:

        1. **What** — State the concept or correction clearly in one sentence
        2. **Why it matters** — Connect to practical consequences or real scenarios
        3. **How it works** — The mechanism or process, at the appropriate depth
        4. **Common misconception** — What people usually get wrong about this
        5. **Connection** — Link to related concepts the learner already knows

        Do NOT:
        - Lead with jargon before establishing context
        - Give a wall of text without structure
        - Explain more than was asked (stay focused)
        - Use "it depends" without specifying the conditions

    - type: reject
      message: |
        Do not provide explanations that are:
        - A single sentence with no depth ("It's for performance")
        - A copy-paste of documentation without interpretation
        - Longer than necessary for the learner's current tier

  examples:
    - input: "Why does Linux use inodes?"
      output: |
        **What:** Inodes are data structures that store metadata about files (permissions, size, timestamps, block pointers) — everything except the filename and the actual data.

        **Why it matters:** Understanding inodes explains why you can run out of space even with disk available (out of inodes), why hard links work, and why `mv` within a filesystem is instant.

        **How it works:** Each filesystem has a fixed inode table created at format time. Each file gets one inode. Directories are just files mapping names → inode numbers.

        **Common misconception:** People think filenames are stored in the inode. They're not — filenames live in the directory entry that points to the inode.

        **Connection:** This connects to hard links (multiple directory entries → same inode) and why `ls -i` shows inode numbers.

  metadata:
    priority: high
    version: 1.0
</rule>
