# Module 3: Process Contracts
# Version: 1.0

Define what each process must achieve -- measurable outcomes, not methods. L2 contracts are executor-invariant.

---

## System Prompt

```
You are a process specification architect using Organizational Schema Theory (OST). Your task is to define process contracts -- measurable outcome specifications that any executor (human, machine, or hybrid) must satisfy.

## Grounding Frameworks

- SIPOC (Six Sigma): Suppliers, Inputs, Process, Outputs, Customers -- ensures each contract has clear boundaries
- ISO 9001:2015 process approach: measurable quality objectives with monitoring and evaluation
- Business Process Management (Dumas et al., 2018): process decomposition and quality gates
- Quality Function Deployment (Akao, 1966): translating customer requirements into measurable process parameters

## Contract-Procedure Separation

In OST, Level 2 contracts are "unit tests" for processes. They specify WHAT the process must achieve, never HOW. This separation is fundamental:

- CONTRACT (L2): "Espresso extraction time must be 25-30 seconds" -- the test
- PROCEDURE (L3): "Grind at setting 7, tamp at 15kg" -- the implementation

Any executor that passes the contract is valid. Contracts do not specify tools, methods, or sequences. This enables:
- Executor substitution (human -> machine -> hybrid) without rewriting contracts
- Fork-and-rewrite: copy the test suite, rewrite the implementation for a different context
- Continuous improvement: optimize the procedure while the contract holds stable

## Quality Gates

Each contract contains quality gates -- measurable parameters with boundaries:
- min: lowest acceptable value
- max: highest acceptable value
- target: ideal value (optional, between min and max)
- description: for qualitative gates that cannot be reduced to a number

Every quality gate must be objectively verifiable. Avoid subjective language ("good", "nice", "appropriate") unless accompanied by a measurable proxy.

## Failure Actions

Each contract specifies what happens when a quality gate is not met. Failure actions must be concrete and actionable:
- What to do with the failed output (discard, rework, escalate)
- What corrective step to take before retrying
- Who to notify if the failure persists

## Linking Convention

Every process contract MUST include a satisfies_signal field linking to one or more L1 signal requirement IDs. This creates backward traceability:
- L2 contract -> satisfies_signal -> L1 signal requirement -> satisfies_experience -> L0 experience contract

Contracts that directly satisfy regulatory or safety constraints should also include satisfies_constraint linking to L0 constraint IDs. Contracts fulfilling brand commitments should include satisfies_commitment linking to L0 commitment IDs.

## Output Format

Produce YAML output with this structure per contract:
1. id: unique identifier (pattern: L2_snake_case_name)
2. satisfies_signal: array of L1 signal requirement IDs this contract serves
3. satisfies_constraint: (optional) array of L0 constraint IDs
4. satisfies_commitment: (optional) array of L0 commitment IDs
5. quality_gates: object of named parameters, each with min/max/target/description
6. failure_action: string describing the concrete response to gate failure
7. owner_role: (optional) the role responsible for contract compliance
```

---

## Context: Upstream Schema Summary

```
Module 1 (L0 Experience Contracts):
- experience_contracts[].id (e.g., L0_product_excellence)
- experience_contracts[].type: experience | constraint | commitment

Module 2 (L1 Signal Requirements):
- signal_requirements[].id (e.g., L1_craft_preparation)
- signal_requirements[].dimension: one of 8 SBT dimensions
- signal_requirements[].satisfies_experience: [L0 IDs]

Every process contract MUST link to at least one L1 signal via satisfies_signal.
Contracts that directly satisfy regulatory constraints should also include satisfies_constraint.
```

---

## Structured Output Template

For consistent, machine-readable results, include the YAML template from `templates/03_process_contracts.yaml` in your prompt and instruct the AI to structure its output accordingly.

The template includes:
- **SIPOC** (Six Sigma) for process boundary definition
- **ISO 9001:2015** process approach for measurable quality objectives
- **QFD** (Akao, 1966) for tracing customer requirements to process parameters
- **Failure action** field per contract for non-conformance response
- **Backward traceability** via satisfies_signal, satisfies_constraint, satisfies_commitment

See `templates/FRAMEWORKS.md` for alternative process specification frameworks.

---

## User Prompt Template

```
Define process contracts for the following business specification:

**Business name**: [NAME]
**L1 Signal Requirements**: [Paste your Module 2 output here]

For each signal requirement, specify the process contracts that will produce it. Focus on measurable quality gates, not methods.

[Optional: Paste the YAML template from templates/03_process_contracts.yaml and add:
"Structure your output using this YAML template."]
```

---

## Example (abbreviated)

**Business**: Spectra Coffee (specialty coffee bar, Berlin)

**L2_espresso_extraction**
- satisfies_signal: [L1_craft_preparation, L1_taste_quality]

| Quality Gate | Min | Max | Target | Description |
|---|---|---|---|---|
| extraction_time_s | 25 | 30 | 27 | Time from pump start to cut |
| temperature_c | 92 | 94 | 93 | Brew water at group head |
| dose_g | 17 | 19 | 18 | Dry coffee in portafilter |
| yield_ml | 34 | 38 | 36 | Liquid espresso in cup |
| crema | -- | -- | -- | Present, golden-brown, minimum 2mm depth |

- failure_action: "Discard shot, recalibrate grinder, re-extract. If three consecutive failures, pull machine offline and descale."
- owner_role: barista

**L2_allergen_management**
- satisfies_constraint: [L0_food_safety_compliance]

| Quality Gate | Min | Max | Target | Description |
|---|---|---|---|---|
| allergen_matrix_current | -- | -- | true | Matrix reviewed and current |
| cross_contamination_incidents | -- | 0 | 0 | Zero tolerance for cross-contamination |
| staff_training_current | -- | -- | true | All staff completed allergen training |

- failure_action: "Halt service of affected product, notify shift lead, update allergen matrix, retrain involved staff within 24 hours."
- owner_role: shift_lead

**L2_customer_greeting**
- satisfies_signal: [L1_warm_greeting]

| Quality Gate | Min | Max | Target | Description |
|---|---|---|---|---|
| greeting_time_s | -- | 15 | 5 | Seconds after customer enters before acknowledgment |
| eye_contact | -- | -- | true | Visual acknowledgment made |
| queue_communication | -- | -- | -- | Wait time communicated if queue exceeds 3 people |

- failure_action: "Coach staff member on greeting protocol during next shift debrief."
- owner_role: barista

**L2_milk_steaming**
- satisfies_signal: [L1_craft_preparation, L1_taste_quality]

| Quality Gate | Min | Max | Target | Description |
|---|---|---|---|---|
| temperature_c | 60 | 65 | 62 | Final milk temperature |
| foam_consistency | -- | -- | -- | Microfoam, no visible bubbles larger than 1mm |
| pour_pattern | -- | -- | -- | Latte art achievable (free pour test) |

- failure_action: "Discard milk, re-steam with fresh milk. If two consecutive failures, recalibrate steam wand pressure."
- owner_role: barista
