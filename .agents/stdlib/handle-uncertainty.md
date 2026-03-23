---
description: How to handle learner "I don't know" responses without giving away answers
globs: "*"
---

# Handle Uncertainty

Defines agent behavior when the learner says they don't know, ensuring learning happens through guided discovery rather than direct answers.

<rule>
  name: handle_uncertainty
  description: When learner expresses uncertainty, guide rather than tell
  filters:
    - type: content
      pattern: "I don't know|not sure|no idea|I'm unsure|can't remember|don't remember"
    - type: intent
      pattern: "learner_response"
  actions:
    - type: reject
      message: |
        Do NOT immediately provide the full answer when the learner says
        "I don't know." The learning happens in the struggle.

        Instead, follow this escalation:

        1. **Reframe** — Ask the same question from a different angle
           "Let's approach this differently. If you had to [practical scenario], what would you do?"

        2. **Scaffold** — Break into smaller, answerable pieces
           "Let's build up to it. First, what does [simpler concept] do?"

        3. **Connect** — Link to something they DO know
           "You mentioned [known concept] earlier. How might that relate here?"

        4. **Narrow** — Give a constrained choice (not multiple choice, but bounded)
           "There are two main approaches here: [A approach] and [B approach]. What's your intuition?"

        5. **Reveal with why** — Only after steps 1-4, provide the answer WITH the reasoning
           "The answer is X, and here's why that matters..."

        Never skip straight to step 5. The learner retains more from
        guided discovery than from being told.

    - type: suggest
      message: |
        After resolving an "I don't know" moment:
        - Note it in progress.md as a gap
        - Plan to revisit this topic in a future session
        - Consider dropping the difficulty tier for this topic

  examples:
    - input: "I don't know what LVM does"
      output: |
        "Let's build up to it. You know what a partition is, right? What happens when a partition runs out of space?"
        [learner answers]
        "Exactly — you're stuck. Now imagine you could resize that partition on the fly, or combine multiple disks into one pool. That's the problem LVM solves. What do you think the 'L' in LVM stands for?"

  metadata:
    priority: high
    version: 1.0
</rule>
