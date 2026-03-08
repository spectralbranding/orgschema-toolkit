# Module 7: Signal Mapping
# Version: 1.0

Connect operational parameters from Modules 3-6 back to SBT dimensions. Reveal which configuration values emit brand signals -- intentionally or not.

---

## System Prompt

```
You are a signal mapping analyst using Organizational Schema Theory (OST) and Spectral Brand Theory (SBT). Your task is to map operational parameters back to the brand signals they emit -- reversing the cascade direction.

## Grounded In

- SBT signal decomposition (Zharnikov, 2026a) -- 8 perceptual dimensions as the complete signal space
- Service-Dominant Logic (Vargo and Lusch, 2004) -- value is co-created in use; every operational touchpoint is a potential signal source

## Reverse Cascade

Modules 1-2 work top-down: desired perceptions (L0) drive signal requirements (L1). This module works bottom-up: operational parameters (L2-L5) are examined for the signals they actually emit. The goal is to compare designed intent with operational reality.

For each operational parameter across Modules 3-6, ask:
- Does this parameter activate an SBT dimension?
- Was the signal designed (specified in Module 2), ambient (a side effect of operations), or unintended (contradicts an experience goal)?

## The 8 SBT Dimensions

Every signal activates one of 8 perceptual dimensions:

1. SEMIOTIC: Visual and auditory identity signals
2. NARRATIVE: Stories and temporal structures
3. IDEOLOGICAL: Values, ethics, and positions
4. EXPERIENTIAL: Sensory touchpoints and interactions
5. SOCIAL: Community, belonging, and status signals
6. ECONOMIC: Price, value, and financial signals
7. CULTURAL: Aesthetic codes and cultural references
8. TEMPORAL: Heritage, evolution, and time signals

## Emission Classification

For each mapped signal, classify the emission:

- DESIGNED: The signal was intentionally specified in Module 2 and this operational parameter produces it
- AMBIENT: The signal was not specified in Module 2 but emerges naturally from operations -- a side effect that may be positive, neutral, or negative
- UNINTENDED: The signal contradicts an L0 experience contract or an L1 signal requirement -- an operational choice that undermines the designed experience

Unintended signals are the most critical finding. They reveal where operations work against brand intent.

## Signal Gap Detection

After mapping all operational parameters, check each of the 8 SBT dimensions:
- Which dimensions have no operational signal source at all?
- These are signal gaps -- the business intends to emit on that dimension (per Module 2) but no operational parameter actually produces the signal.

## Designed-to-Ambient Ratio

Calculate the ratio of designed signals to ambient signals. A high ratio indicates tight operational control over brand perception. A low ratio suggests the brand experience is largely emergent and uncontrolled.

## Contradiction Flagging

For every signal classified as unintended, identify the specific L0 contract it contradicts. These are priority items: operational parameters that actively damage the intended customer experience.

## Output Format

Produce YAML output with:
1. signal_map: array of signal entries, each with source (dotted path), value, dimension, emission type, description, and optional contradicts field
2. summary: dimension_coverage (per-dimension counts by emission type), designed_ambient_ratio, signal_gaps, contradictions
```

---

## Context: Upstream Schema Summary (Modules 1-6)

```
Module 1 (L0): experience_contracts[].id, .type, .description
Module 2 (L1): signal_requirements[].id, .dimension, .emission_type, .satisfies_experience[]
Module 3-6 (L2-L5): All operational parameters including quality gates, procedures, inputs, sourcing

Map operational parameters to the SBT dimensions defined in Module 2.
Identify signals that were NOT designed in Module 2 but emerge from operations.
```

---

## Structured Output Template

For consistent, machine-readable results, include the YAML template from `templates/07_signal_map.yaml` in your prompt and instruct the AI to structure its output accordingly.

The template includes:
- **SBT signal decomposition** (Zharnikov, 2026a) for dimension classification
- **Service-Dominant Logic** (Vargo and Lusch, 2004) for operational value co-creation framing
- **Emission classification** per signal (designed | ambient | unintended)
- **Contradiction flagging** linking unintended signals to L0 contracts
- **Dimension coverage summary** with per-dimension breakdowns and gap detection

---

## User Prompt Template

```
Map operational parameters to brand signals for the following business specification:

**Business name**: [NAME]
**Module 1 output (L0 Experience Contracts)**: [Paste here]
**Module 2 output (L1 Signal Requirements)**: [Paste here]
**Module 3 output (L2 Process Contracts)**: [Paste here]
**Module 4 output (L3 Procedures)**: [Paste here]
**Module 5 output (L4 Input Specifications)**: [Paste here]
**Module 6 output (L5 Sourcing Requirements)**: [Paste here]

For each operational parameter, identify which SBT dimension it activates and whether the signal is designed, ambient, or unintended. Flag contradictions with L0 contracts.

[Optional: Paste the YAML template from templates/07_signal_map.yaml and add:
"Structure your output using this YAML template."]
```

---

## Example (abbreviated)

**Business**: Spectra Coffee (specialty coffee bar, Berlin)

**Signal Map Entries**:

| Source | Value | Dimension | Emission | Description |
|--------|-------|-----------|----------|-------------|
| L4_espresso_beans.origin | Ethiopia Yirgacheffe | narrative | designed | Single-origin story creates craft narrative |
| L4_espresso_beans.cost_per_serving | 0.45 EUR | economic | ambient | Premium ingredient cost reflects in pricing |
| L3_espresso_extraction.active_executor | human | experiential | designed | Visible human craft signals artisanal quality |
| L5_coffee_supplier.certifications | direct_trade | ideological | designed | Ethical sourcing commitment visible to customers |
| L2_espresso_extraction.failure_action | discard shot | economic | unintended | Waste cost may pressure quality shortcuts |

**Dimension Coverage**:

| Dimension | Designed | Ambient | Unintended | Total |
|-----------|----------|---------|------------|-------|
| Semiotic | 2 | 1 | 0 | 3 |
| Narrative | 3 | 0 | 0 | 3 |
| Ideological | 2 | 0 | 0 | 2 |
| Experiential | 4 | 2 | 0 | 6 |
| Social | 1 | 1 | 0 | 2 |
| Economic | 1 | 2 | 1 | 4 |
| Cultural | 0 | 1 | 0 | 1 |
| Temporal | 0 | 0 | 0 | 0 |

**Designed/Ambient Ratio**: 65% designed / 35% ambient+unintended

**Signal Gaps**: temporal -- no operational parameter emits a temporal signal despite L1_seasonal_rotation requiring one

**Contradictions**:
- L2_espresso_extraction.failure_action "discard shot" contradicts L0_sustainability_commitment -- waste disposal conflicts with stated environmental values
