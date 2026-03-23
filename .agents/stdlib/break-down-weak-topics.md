---
description: When learner scores below threshold, break topic into sub-topics automatically
globs: "*"
---

# Break Down Weak Topics

When a learner consistently scores poorly on a topic, the agent should decompose it into smaller, more manageable sub-topics rather than repeating the same questions.

<rule>
  name: break_down_weak_topics
  description: Decompose topics into sub-topics when learner scores are persistently low
  filters:
    - type: content
      pattern: "score.*[12]/5|Weak|Needs Work|persistent weakness"
    - type: intent
      pattern: "generate_quiz|select_topic|update_progress"
  actions:
    - type: suggest
      message: |
        When a topic has score < 3/5 across 2+ sessions:

        1. **Decompose** — Break the topic into 3-5 sub-topics
           Example: "Networking" → "IP addressing", "DNS resolution",
           "TCP handshake", "routing tables", "firewall rules"

        2. **Find the root** — Identify which sub-topic is the actual
           gap. Often a weak "Networking" score is really a weak
           "DNS resolution" understanding dragging everything down.

        3. **Drop tier** — Move sub-topics to Tier 1 (foundational).
           Build back up from there.

        4. **Update progress.md** — Replace the parent topic entry
           with sub-topic entries so the learning loop targets them
           individually.

        5. **Reconnect** — Once sub-topics are at Tier 2+, generate
           a synthesis question that ties them back together.

        This follows from Huntley's "don't assume not implemented" —
        don't assume the whole topic is weak. Find the specific gap.

    - type: reject
      message: |
        Do not keep asking the same Tier 3-4 questions on a topic
        where the learner has scored below 3/5 twice. That's wasted
        backpressure — you're testing at a level they haven't reached.

  examples:
    - input: "Storage Fundamentals: 2.1/5 across 3 quizzes"
      output: |
        Breaking "Storage Fundamentals" into sub-topics:
        - Block devices and partitions (Tier 1)
        - Filesystem types and mounting (Tier 1)
        - LVM concepts and operations (Tier 1)
        - RAID levels and trade-offs (Tier 1)
        - Storage troubleshooting (Tier 2, after foundations)

        Updated progress.md with sub-topic entries.
        Next quiz will target "Block devices and partitions" at Tier 1.

  metadata:
    priority: high
    version: 1.0
</rule>
