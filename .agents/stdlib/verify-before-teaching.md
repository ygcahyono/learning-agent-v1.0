---
description: Always verify technical claims before presenting them as fact to the learner
globs: "*"
---

# Verify Before Teaching

Prevents the agent from teaching incorrect information. Directly sourced from Huntley's "if you find inconsistencies in the specs then use the oracle and update the specs."

<rule>
  name: verify_before_teaching
  description: Verify technical claims against authoritative sources before teaching
  filters:
    - type: intent
      pattern: "explain|teach|generate_reading|generate_quiz|grade"
    - type: content
      pattern: "command|syntax|config|flag|option|parameter|version"
  actions:
    - type: suggest
      message: |
        Before presenting technical information as fact:

        1. **Commands** — Verify flags and syntax are correct for the
           target OS/version. Don't teach `ls -la --color` if the
           learner is on a system where `--color` isn't supported.

        2. **Configuration** — Verify file paths, directive names, and
           default values against current documentation.

        3. **Behavior** — Verify that described behavior matches reality.
           "Process X happens before Y" should be confirmed, not assumed.

        4. **Version-specific** — Note when behavior differs across
           versions. Don't present version-specific behavior as universal.

        If you cannot verify a claim:
        - Mark it explicitly: "I believe X but have not verified this"
        - Do NOT present it as fact
        - Flag for learner to verify themselves

    - type: reject
      message: |
        Do not present unverified technical claims as fact.
        Do not teach commands you haven't verified exist.
        Do not describe behavior you're inferring rather than confirming.

  examples:
    - input: "The default SSH port is 22 and it uses /etc/ssh/sshd_config"
      output: "Verified: SSH default port 22 is correct (RFC 4253). Config path /etc/ssh/sshd_config is correct for most Linux distributions."

    - input: "Use systemctl reload nginx to apply config changes"
      output: "Verified: systemctl reload sends SIGHUP to nginx, which reloads config without dropping connections. This is preferred over restart for config changes."

  metadata:
    priority: critical
    version: 1.0
</rule>
