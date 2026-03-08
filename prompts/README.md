# Organizational Schema Theory: AI Prompt Toolkit

**Version**: 1.0
**Last Updated**: 2026-03-08

---

## What This Is

A set of 8 structured prompts for specifying a complete business using the Organizational Schema Theory framework. Each module is a self-contained prompt that works with Claude, GPT, or any capable LLM.

Each module includes:
- **System prompt** with methodology and instructions
- **Context** section with condensed upstream schema (so the LLM produces structurally correct output)
- **User prompt template** with fill-in fields
- **Output template** (YAML) for structured, machine-readable results

## How to Use

1. Start with **Module 1** (Experience Design) -- it requires zero framework knowledge
2. Cascade through Modules 2-6 sequentially (each takes ~5 minutes)
3. Run **Module 7** (Signal Mapping) to connect operations to brand perception
4. Run **Module 8** (Gap Analysis) to validate the complete specification

### With Output Templates

For each module, include the corresponding YAML template from `templates/` in your prompt. Tell the AI: "Structure your output using the provided YAML template." This ensures:
- Consistent output across sessions
- Machine-readable results that feed into the next module
- Correct field names for cross-reference linking

Templates are optional but recommended. Without them, the AI produces free-form analysis. With them, you get structured data compatible with the [orgschema-validate](https://github.com/spectralbranding/orgschema-framework) CLI.

### With Starter Packs

For common business types, copy a starter pack from `starters/` instead of starting from scratch. Starter packs provide pre-filled L0 experience contracts. Modify them for your specific business, then proceed to Module 2.

## Modules

| Module | Prompt | Template | Purpose | Input |
|:-------|:-------|:---------|:--------|:------|
| 1 | `01_EXPERIENCE_DESIGN.md` | `01_experience_contracts.yaml` | Define customer experience goals, regulations, commitments | Business description (plain language) |
| 2 | `02_SIGNAL_ARCHITECTURE.md` | `02_signal_requirements.yaml` | Specify signals across 8 SBT dimensions | Module 1 output |
| 3 | `03_PROCESS_CONTRACTS.md` | `03_process_contracts.yaml` | Define measurable process outcomes | Module 2 output |
| 4 | `04_PROCEDURE_SPECIFICATION.md` | `04_procedures.yaml` | Specify step-by-step executor procedures | Module 3 output |
| 5 | `05_INPUT_SPECIFICATION.md` | `05_input_specifications.yaml` | Define materials, equipment, capabilities | Module 4 output |
| 6 | `06_SOURCING_REQUIREMENTS.md` | `06_sourcing_requirements.yaml` | Specify supplier criteria and logistics | Module 5 output |
| 7 | `07_SIGNAL_MAPPING.md` | `07_signal_map.yaml` | Map operations back to brand signals | Modules 1-6 output |
| 8 | `08_GAP_ANALYSIS.md` | `08_validation_report.yaml` | Validate completeness and traceability | All module outputs |

## Module Dependencies

```
Module 1 (L0)
    |
    v
Module 2 (L1)
    |
    v
Module 3 (L2)
    |
    v
Module 4 (L3)
    |
    v
Module 5 (L4)
    |
    v
Module 6 (L5)
    |
    v
Module 7 (Signal Map) -----> [connects back to Module 2]
    |
    v
Module 8 (Validation)
```

Modules 1-6 form the core cascade (top-down design). Module 7 connects operations to brand perception. Module 8 validates the complete specification.

## Frameworks Used Per Module

| Module | Primary Framework | Secondary |
|:-------|:-----------------|:----------|
| 1 | Design Thinking, Jobs-to-Be-Done | Service Blueprinting, Backward Design |
| 2 | SBT 8 Dimensions | Service Design |
| 3 | SIPOC, ISO 9001 | BPM, QFD |
| 4 | Toyota Standardized Work | TWI Job Instruction, Cognitive Task Analysis |
| 5 | Bill of Materials (MRP/ERP) | ISO 22000, Supply Chain Management |
| 6 | ISO 9001:2015 clause 8.4 | Supplier Relationship Management |
| 7 | SBT Signal Decomposition | Service-Dominant Logic |
| 8 | TDD, Lean Muda Analysis | Value Stream Mapping, FMEA |

See `templates/FRAMEWORKS.md` for alternative framework citations per module.
