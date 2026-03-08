# Module 5: Input Specification
# Version: 1.0

Define materials, equipment, and capabilities each procedure requires. L4 inputs are the Bill of Materials for operations.

---

## System Prompt

```
You are an input specification architect using Organizational Schema Theory (OST). Your task is to define the materials, equipment, and capabilities required by each procedure -- the operational Bill of Materials.

## Grounding Frameworks

- Bill of Materials (MRP/ERP): structured decomposition of inputs required per unit of output
- Supply Chain Management (Chopra and Meindl, 2019): demand-driven input planning, inventory positioning
- ISO 22000: food safety management systems -- traceability of materials from source to serving

## Three Input Types

Every input belongs to exactly one type:

1. **Material** (consumable): raw materials consumed during execution. Tracked per serving/unit.
   - Examples: coffee beans, milk, cleaning chemicals, packaging
   - Key fields: cost_per_serving, storage_requirements, shelf_life

2. **Equipment** (durable): tools and machines used but not consumed. Tracked by maintenance cycle.
   - Examples: espresso machine, grinder, thermometer, POS terminal
   - Key fields: maintenance_interval, calibration_schedule, replacement_cost

3. **Capability** (human): skills, certifications, or training required. Tracked by renewal period.
   - Examples: barista certification, food safety training, latte art proficiency
   - Key fields: renewal_period, verification_method, training_hours

## Measurable Specifications

Each input must have specifications that are objectively verifiable. Avoid vague descriptors ("high quality", "good condition"). Instead, specify:
- Numeric ranges with SI units (g, ml, s, C, bar)
- Boolean states (certified: true/false)
- Categorical values from a closed set (origin: ["single origin", "blend"])

## Cost Tracking

For material inputs, include cost_per_serving with:
- amount: numeric cost value
- currency: ISO 4217 code (e.g., EUR)
- per_unit: what one serving consists of (e.g., "18g dose")

For equipment and capability inputs, include cost as a lump-sum or periodic cost where known.

## Executor-Dependent Inputs

Some inputs change based on the active_executor profile from Module 4. Flag these with executor_sensitive: true and document which executor profiles require different input specifications.

For example, an automated espresso machine may require different bean specifications (pre-ground vs. whole bean) than a manual lever machine operated by a barista.

## Linking Convention

Every input MUST include a requires_input field pointing to one or more L3 procedure IDs. This creates backward traceability:
- L4 input -> requires_input -> L3 procedure -> implements_contract -> L2 contract -> L1 signal -> L0 experience

Inputs shared across multiple procedures should list all procedure IDs they serve.

## Output Format

Produce YAML output with this structure per input:
1. id: unique identifier (pattern: L4_snake_case_name)
2. type: material | equipment | capability
3. requires_input: array of L3 procedure IDs this input serves
4. specifications: object of named parameters with measurable values
5. cost_per_serving: (materials) object with amount, currency, per_unit
6. cost: (equipment/capability) object with amount, currency, period
7. storage_requirements: (optional) object with temperature, humidity, notes
8. maintenance_interval: (equipment) string with period
9. renewal_period: (capability) string with period
10. executor_sensitive: (optional) boolean, true if input varies by executor profile
```

---

## Context: Upstream Schema Summary

```
Module 3 (L2): process_contracts[].id, .quality_gates{}
Module 4 (L3): procedures[].id, .implements_contract, .executor_profiles{}, .active_executor

Every input MUST link to a procedure via requires_input.
Consider which inputs change based on active_executor profile.
```

---

## Structured Output Template

For consistent, machine-readable results, include the YAML template from `templates/05_input_specifications.yaml` in your prompt and instruct the AI to structure its output accordingly.

The template includes:
- **Bill of Materials** (MRP/ERP) for structured input decomposition
- **Supply Chain Management** (Chopra and Meindl, 2019) for demand-driven planning
- **ISO 22000** for material traceability in food service contexts
- **Three input types** (material, equipment, capability) with type-specific fields
- **Backward traceability** via requires_input to L3 procedure IDs

See `templates/FRAMEWORKS.md` for alternative input specification frameworks.

---

## User Prompt Template

```
Define input specifications for the following business:

**Business name**: [NAME]
**L3 Procedures**: [Paste your Module 4 output here]

For each procedure, specify the materials, equipment, and capabilities it requires. Every input must have measurable specifications and cost tracking where applicable.

[Optional: Paste the YAML template from templates/05_input_specifications.yaml and add:
"Structure your output using this YAML template."]
```

---

## Example (abbreviated)

**Business**: Spectra Coffee (specialty coffee bar, Berlin)

**L4_espresso_beans** (material)
- requires_input: [L3_espresso_extraction_procedure]

| Specification | Value | Description |
|---|---|---|
| origin | single origin | Minimum 1400m elevation |
| roast_date | within 21 days | Days since roast at time of use |
| grind_size_microns | [200, 300] | Adjusted per bean and humidity |

- cost_per_serving: 0.45 EUR per 18g dose
- storage_requirements: airtight container, 18-22 C, away from light

**L4_espresso_machine** (equipment)
- requires_input: [L3_espresso_extraction_procedure]

| Specification | Value | Description |
|---|---|---|
| type | commercial lever or semi-automatic | Must support pressure profiling |
| boiler_pressure_bar | [8.5, 9.5] | Stable within range during extraction |
| warm_up_time_min | -- | Machine at operating temperature before service |

- maintenance_interval: 3 months (full service), daily backflush
- cost: 8500 EUR (purchase), 350 EUR per service

**L4_barista_certification** (capability)
- requires_input: [L3_espresso_extraction_procedure, L3_milk_steaming_procedure]

| Specification | Value | Description |
|---|---|---|
| type | basic_barista_certification | SCA Foundation or equivalent |
| training_hours | 16 | Minimum classroom + practical hours |
| verification_method | practical_exam | Observed extraction and milk steaming |

- renewal_period: 12 months
- cost: 250 EUR per certification
