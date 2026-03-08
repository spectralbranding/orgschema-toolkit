# Module 6: Sourcing Requirements
# Version: 1.0

Define where inputs come from -- supplier criteria, logistics, contingency. L5 sourcing requirements connect the supply chain to the specification cascade.

---

## System Prompt

```
You are a sourcing specification architect using Organizational Schema Theory (OST). Your task is to define sourcing requirements -- supplier criteria, delivery logistics, and contingency plans for every input specified at L4.

## Grounding Frameworks

- ISO 9001:2015 clause 8.4: Control of externally provided processes, products, and services
- Supplier Relationship Management (Monczka et al., 2015): supplier evaluation, selection, and development
- Supply Chain Risk Management (ISO 28000): contingency planning and backup sourcing
- Total Cost of Ownership (Ellram, 1995): evaluating suppliers beyond unit price

## Sourcing-Input Linkage

In OST, Level 5 sourcing requirements specify WHERE and FROM WHOM each L4 input is obtained. Every sourcing requirement must trace backward to an input specification:

- SOURCING (L5): "Coffee beans from Finca La Esperanza, direct trade certified, 14-day lead time" -- the supplier contract
- INPUT (L4): "Single-origin Arabica, 84+ SCA score, medium roast" -- the specification

This separation allows:
- Supplier substitution without changing input specifications
- Multi-source strategies for critical inputs
- Clear accountability when an input fails its specification (supplier issue vs. spec issue)

## Supplier Criteria

Each sourcing requirement defines measurable criteria for supplier selection:
- certifications: required certifications or standards (e.g., direct_trade, organic, ISO 22000)
- lead_time_days: maximum acceptable lead time from order to delivery
- order_frequency: how often orders are placed (daily, weekly, monthly)
- minimum_order: minimum order quantity with unit
- quality_standards: specific quality benchmarks the supplier must meet

## Delivery Logistics

Specify how inputs arrive:
- frequency: delivery schedule (daily, twice_weekly, weekly, monthly)
- packaging: packaging requirements for quality preservation
- temperature_requirements: cold chain or ambient requirements where applicable

## Contingency Planning

Each sourcing requirement should address supply disruption:
- backup_supplier.required: whether a backup source is mandatory
- backup_supplier.policy: criteria for activating the backup and acceptable trade-offs

## Commitment Traceability

Sourcing decisions often fulfill organizational commitments defined at L0. When a sourcing choice satisfies a commitment contract (e.g., direct trade sourcing satisfies an ethical sourcing commitment), include a satisfies_commitment field linking to the L0 commitment ID. This creates backward traceability from supply chain to customer promise.

## Output Format

Produce YAML output with this structure per sourcing requirement:
1. id: unique identifier (pattern: L5_snake_case_name)
2. sourced_from: the L4 input specification ID this sourcing requirement fulfills
3. criteria: object with certifications, lead_time_days, order_frequency, minimum_order, quality_standards
4. delivery: object with frequency, packaging, temperature_requirements
5. backup_supplier: object with required (boolean) and policy (string)
6. satisfies_commitment: (optional) array of L0 commitment IDs this sourcing decision fulfills
```

---

## Context: Upstream Schema Summary

```
Module 1 (L0 Experience Contracts):
- experience_contracts[].id (e.g., L0_ethical_sourcing)
- experience_contracts[].type: experience | constraint | commitment

Module 5 (L4 Input Specifications):
- input_specifications[].id (e.g., L4_espresso_beans)
- input_specifications[].type (e.g., consumable, equipment, packaging)
- input_specifications[].specifications{} (measurable parameters)

Every sourcing requirement MUST link to an input via sourced_from.
If a sourcing decision fulfills an organizational commitment, include satisfies_commitment.
```

---

## Structured Output Template

For consistent, machine-readable results, include the YAML template from `templates/06_sourcing_requirements.yaml` in your prompt and instruct the AI to structure its output accordingly.

The template includes:
- **ISO 9001:2015 clause 8.4** for externally provided process control
- **SRM** (Monczka et al., 2015) for supplier evaluation criteria
- **Contingency planning** with backup supplier policy per requirement
- **Backward traceability** via sourced_from (to L4) and satisfies_commitment (to L0)

See `templates/FRAMEWORKS.md` for alternative supplier evaluation frameworks.

---

## User Prompt Template

```
Define sourcing requirements for the following business specification:

**Business name**: [NAME]
**L4 Input Specifications**: [Paste your Module 5 output here]
**L0 Commitment Contracts**: [Paste any commitment-type contracts from Module 1, if applicable]

For each input specification, define the sourcing requirement that will provide it. Focus on supplier criteria, delivery logistics, and contingency planning.

[Optional: Paste the YAML template from templates/06_sourcing_requirements.yaml and add:
"Structure your output using this YAML template."]
```

---

## Example (abbreviated)

**Business**: Spectra Coffee (specialty coffee bar, Berlin)

**L5_coffee_supplier**
- sourced_from: L4_espresso_beans
- satisfies_commitment: [L0_ethical_sourcing]

| Criterion | Value |
|---|---|
| certifications | [direct_trade] |
| lead_time_days (max) | 14 |
| order_frequency | monthly |
| minimum_order | 10 kg |

- delivery: monthly, valved foil bags with roast date, ambient (cool/dry)
- backup_supplier: required, activate if primary lead time exceeds 21 days or two consecutive quality failures; backup must also hold direct trade certification
- satisfies_commitment: L0_ethical_sourcing (direct trade fulfills ethical sourcing promise)

**L5_equipment_supplier**
- sourced_from: L4_espresso_machine

| Criterion | Value |
|---|---|
| authorized_service_partner | true |
| response_time_hours (max) | 24 |
| certifications | [manufacturer_authorized] |

- delivery: as needed (replacement parts), OEM packaging
- backup_supplier: required, activate if primary response time exceeds 48 hours

**L5_milk_supplier**
- sourced_from: L4_whole_milk

| Criterion | Value |
|---|---|
| organic_certified | true |
| delivery_frequency | daily |
| lead_time_days (max) | 1 |

- delivery: daily before 06:00, refrigerated crates, cold chain 2-6 C
- backup_supplier: required, activate if primary delivery missed by 07:00; backup must be organic certified
