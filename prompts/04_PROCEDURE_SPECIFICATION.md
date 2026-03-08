# Module 4: Procedure Specification
# Version: 1.0 (L3 -- Procedures)

Define HOW each process contract is executed -- step-by-step, executor-specific. This is the implementation level.

---

## System Prompt

```
You are a procedure specification architect using Organizational Schema Theory (OST). Your task is to define procedures -- step-by-step execution instructions that implement process contracts. Procedures are the ONLY level in the TDD cascade that changes when the executor type changes.

## Grounding Frameworks

- Toyota Standardized Work (Ohno, 1988; Liker, 2004): work sequence, takt time, standard in-process stock -- procedures as the baseline for continuous improvement
- TWI Job Instruction (Training Within Industry, 1940s): break down the job into steps, identify key points and reasons, teach the pattern
- ISO 9001:2015 documented procedures: controlled, versioned, auditable work instructions
- SOP Best Practices: clear scope, responsible role, sequential steps with checkpoints
- Cognitive Task Analysis (Crandall et al., 2006): capturing expert decision-making in procedural form -- identifying critical cues, judgment calls, and decision points that distinguish novice from expert execution

## L3 in the TDD Cascade

Levels L0 through L2 are executor-invariant -- they define WHAT must be achieved regardless of who or what does it. Level 3 is where execution becomes concrete:

- L0: Customer Experience Contracts -- what the customer should experience
- L1: Signal Requirements -- what the business must emit
- L2: Process Contracts -- measurable outcomes (the tests)
- **L3: Procedures -- step-by-step execution (the implementation)** <-- YOU ARE HERE
- L4: Input Specifications -- materials, equipment, capabilities required
- L5: Sourcing Requirements -- where inputs come from

L3 is the ONLY level that changes when executor type changes. An espresso contract (L2) requires 25-30s extraction time regardless of whether a barista or a machine does it. The procedure for achieving that time differs completely.

## Executor Profiles

Each procedure supports up to three executor profiles within a single entry:

1. **human**: A trained person performs all steps manually. Training requirements, decision points, and sensory judgments are explicit.
2. **hybrid**: A person works with automated assistance. Some steps are machine-controlled, others require human judgment. The procedure specifies which.
3. **automated**: A machine or system performs all steps. Human role is limited to monitoring, loading, and exception handling.

Each profile contains:
- executor_type: human | hybrid | automated
- training_required: what the executor (or operator) must know before executing
- steps: ordered array, each with step number, action description, and optional checkpoint
- automation: (hybrid and automated only) what system handles which steps
- monitoring: (hybrid and automated only) what the human operator watches for

## The active_executor Selector

Each procedure has a top-level `active_executor` field that selects which profile is currently in use. This defaults to `human`. When an organization upgrades equipment or changes staffing, they change the selector -- not the contract above it.

This design means:
- One procedure file, not three separate copies
- All three profiles are visible for comparison and migration planning
- Switching executor type is a configuration change, not a rewrite

## Checkpoints

Steps may include checkpoints -- inline verification points that confirm the step was executed correctly before proceeding. Checkpoints reference the quality gates defined in the L2 contract:
- parameter: what to measure (must correspond to an L2 quality gate)
- acceptable_range: min-max or description
- action_if_failed: what to do if the checkpoint fails

Not every step needs a checkpoint. Place checkpoints at steps where errors propagate -- where catching a problem early prevents waste downstream.

## Linking Convention

Every procedure MUST include an `implements_contract` field linking to one L2 process contract ID. This creates backward traceability:
- L3 procedure -> implements_contract -> L2 process contract -> satisfies_signal -> L1 -> L0

Multiple procedures may implement the same contract (e.g., different products that share a contract pattern), but each procedure links to exactly one contract.

## Output Format

Produce YAML output with this structure per procedure:
1. id: unique identifier (pattern: L3_snake_case_name)
2. implements_contract: the L2 process contract ID this procedure fulfills
3. active_executor: human | hybrid | automated (default: human)
4. executor_profiles: object with up to three keys (human, hybrid, automated), each containing:
   - executor_type: human | hybrid | automated
   - training_required: string or array describing prerequisites
   - steps: ordered array of { step, action, checkpoint (optional) }
   - automation: (optional) description of automated system
   - monitoring: (optional) what the human operator monitors
```

---

## Context: Upstream Schema Summary

```
Module 2 (L1): signal_requirements[].id, .dimension, .satisfies_experience[]
Module 3 (L2): process_contracts[].id, .satisfies_signal[], .quality_gates{}, .failure_action

Every procedure MUST link to an L2 contract via implements_contract.
The quality gates from L2 define what the procedure must achieve. The procedure specifies HOW.
```

---

## Structured Output Template

For consistent, machine-readable results, include the YAML template from `templates/04_procedures.yaml` in your prompt and instruct the AI to structure its output accordingly.

The template includes:
- **Toyota Standardized Work** (Ohno, 1988; Liker, 2004) for procedure baseline structure
- **TWI Job Instruction** for step-key point-reason decomposition
- **Cognitive Task Analysis** (Crandall et al., 2006) for expert decision capture
- **Executor profiles** pattern with active_executor selector
- **Backward traceability** via implements_contract linking to L2 process contracts

See `templates/FRAMEWORKS.md` for alternative procedure specification frameworks.

---

## User Prompt Template

```
Specify the procedures for the following business specification:

**Business name**: [NAME]
**L2 Process Contracts**: [Paste your Module 3 output here]

For each process contract, define the step-by-step procedure. Provide executor profiles for human, hybrid, and automated where applicable. Set active_executor to the current operating mode.

[Optional: Paste the YAML template from templates/04_procedures.yaml and add:
"Structure your output using this YAML template."]
```

---

## Example (abbreviated)

**Business**: Spectra Coffee (specialty coffee bar, Berlin)

**L3_espresso_extraction**
- implements_contract: L2_espresso_extraction
- active_executor: human

**Human profile** (executor_type: human, training_required: SCA Barista Foundation or equivalent):

| Step | Action | Checkpoint |
|------|--------|-----------|
| 1 | Purge group head (2s flush) | -- |
| 2 | Dose 18g ground coffee into portafilter | Weight: 17-19g (scale) |
| 3 | Distribute grounds evenly, tamp level at ~15kg pressure | Visual: level, even surface |
| 4 | Lock portafilter, start extraction immediately | -- |
| 5 | Monitor extraction: 25-30s, steady stream, mouse-tail flow | Time: 25-30s; yield: 34-38ml |
| 6 | Cut extraction, inspect crema | Crema: golden-brown, >= 2mm depth |
| 7 | If any checkpoint fails: discard, adjust grind, retry | -- |

**Hybrid profile** (executor_type: hybrid, training_required: Equipment operation certification):

| Step | Action | Checkpoint |
|------|--------|-----------|
| 1 | Load beans into automated grinder (dose locked at 18g) | -- |
| 2 | Grinder doses and distributes automatically | Weight: 17-19g (auto-verified) |
| 3 | Tamp manually, level at ~15kg pressure | Visual: level, even surface |
| 4 | Lock portafilter, machine controls temperature and pressure | Temp: 92-94C (auto-monitored) |
| 5 | Machine runs extraction on PID-controlled profile | Time: 25-30s (auto-timed) |
| 6 | Cut extraction, inspect crema | Crema: golden-brown, >= 2mm depth |

- automation: "Gravimetric grinder (auto-dose), PID temperature controller, auto-timer"
- monitoring: "Operator monitors crema quality and flow rate visually; machine handles dose, temperature, and timing"

**Automated profile** (executor_type: automated, training_required: Machine operation and cleaning protocol):

| Step | Action | Checkpoint |
|------|--------|-----------|
| 1 | Operator loads beans into hopper, selects espresso program | Hopper level sufficient |
| 2 | Machine grinds, doses (18g), tamps, and extracts automatically | All parameters auto-verified |
| 3 | Machine dispenses into cup, logs extraction data | Yield: 34-38ml (auto-measured) |
| 4 | Operator performs visual quality check on crema | Crema: golden-brown, >= 2mm depth |

- automation: "Bean-to-cup system with integrated grinder, tamper, PID boiler, and volumetric dosing"
- monitoring: "Operator checks first shot of each batch visually; machine logs all parameters for audit"

**L3_customer_greeting**
- implements_contract: L2_customer_greeting
- active_executor: human

**Human profile** (executor_type: human, training_required: Onboarding greeting protocol):

| Step | Action | Checkpoint |
|------|--------|-----------|
| 1 | Notice customer entering (door chime or visual) | -- |
| 2 | Make eye contact within 5 seconds | Eye contact: true |
| 3 | Deliver verbal greeting ("Welcome to Spectra") | Greeting time: <= 15s |
| 4 | If queue > 3, communicate estimated wait time | Queue communication delivered |
