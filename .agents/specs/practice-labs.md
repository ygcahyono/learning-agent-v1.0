# Spec: Practice Labs

> **Source fidelity: 35%** — The X post confirms practice labs exist ("it told me to setup LVM stuff on a GCP VM, which is kind of cool"). Huntley's "loop back" principle (loop the agent back on itself for evaluation) maps to verification steps. But the specific lab types, generation rules, file format, and verification approach are largely my design.

## Purpose

Practice labs are the hands-on component. Quizzes test knowledge; labs test the ability to DO things. This is backpressure through execution.

## What's Confirmed

From the X post:
- The system generates hands-on practice ✅
- Example given: setting up LVM on a GCP VM ✅
- The author "didn't do much since I'm too short on time" (labs are optional/time-dependent) ✅

From Huntley (ghuntley.com/ralph/):
- "Always look for opportunities to loop Ralph back on itself. This could be as simple as instructing it to add additional logging, or in the case of a compiler, asking Ralph to compile the application and then looking at the LLVM IR representation."
  → Learning equivalent: don't just ask about LVM — have the learner set it up and verify the results.

## Lab Types

<!-- YOUR INPUT NEEDED
The four lab types below are my design. The X post only confirms
that guided setup-style labs were generated. The other types
(scenario, troubleshooting, design) are my extrapolation.

You should decide:
- Which lab types make sense for your learning topics
- Whether you even want multiple types or just one
- Whether labs should be mandatory or optional in the loop
-->

### Type 1: Guided Setup
**When:** New to a topic (Tier 1-2)
**Format:** Step-by-step with verification checkpoints
**Example:** "Set up an LVM volume group. Verify with these commands."

### Type 2: Scenario-Based
**When:** Foundational knowledge (Tier 2-3)
**Format:** Requirements given, approach left to learner
**Example:** "A server needs expandable storage. Set it up."

### Type 3: Troubleshooting
**When:** Good knowledge (Tier 3-4)
**Format:** Broken environment, diagnose and fix
**Example:** "Server can't mount /data after reboot. Fix it."

### Type 4: Design Challenge
**When:** Approaching mastery (Tier 4)
**Format:** Open-ended design with constraints
**Example:** "Design storage for a 3-node cluster with redundancy."

## Verification (Backpressure)

Labs MUST include verification steps. Without verification, there is no backpressure.

From Huntley's backpressure philosophy: the agent must be able to evaluate whether the work was done correctly. For labs, this means:

- Every task includes a command to verify the result
- Expected output is documented so the learner can self-check
- If the learner shares terminal output, the agent can verify

```markdown
**Verification:**
```bash
# Run this to verify
sudo lvs --noheadings -o lv_name,vg_name,lv_size
# Expected: my_lv  my_vg  100.00g
```
```

## Lab File Format

<!-- YOUR INPUT NEEDED
This entire format is my design. The original author's practice
files (01-linux-practice.md, 02-storage-practice.md, etc.) are
not publicly available. Adjust or simplify as needed.
-->

Saved to `practice/`:

```markdown
# Practice Lab: {Title}

## Objective
{What the learner will accomplish}

## Prerequisites
- {Required knowledge}
- {Required environment}

## Environment Setup
{How to set up the environment}

## Tasks

### Task 1: {Description}
{Instructions}

**Verification:**
{command and expected output}

### Task 2: {Description}
...

## Verification Checklist
- [ ] {Outcome 1}
- [ ] {Outcome 2}
```

<!-- YOUR INPUT NEEDED
Major open questions for this spec:

- Environment: Do you want labs that require cloud VMs (GCP/AWS),
  local Docker, or paper/whiteboard exercises? The original used GCP.

- Integration with learning loop: Should labs be required before a
  topic can reach "Strong" status? Or purely optional?

- Time investment: Labs take significantly more time than quizzes.
  How do you want to balance the two?

- Scope: For data science topics (vs. infrastructure), labs might
  look very different — Jupyter notebooks, SQL exercises, model
  building. The current spec is infrastructure-oriented since
  that's what the original X post described.
-->
