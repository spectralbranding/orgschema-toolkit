# Organizational Schema Theory: Methodology Summary

**Version**: 1.0
**Full paper**: [Preprint (DOI)](https://doi.org/10.5281/zenodo.18946043)

---

## Core Concept

Organizational Schema Theory applies Test-Driven Development methodology to business design. Businesses are designed **backward** from desired customer experience through testable, version-controlled specifications where each operational layer validates the layer above it.

This is not a format choice (YAML vs. Word). It is a design methodology that produces businesses that are **testable, forkable, and traceable** from every operational parameter back to its customer-experience justification.

## The TDD Cascade

Six levels, designed top-down (perception to sourcing), operated bottom-up (sourcing to perception):

```
L0: Customer Experience Contract  ->  "acceptance tests"
L1: Signal Requirements           ->  "integration tests"
L2: Process Contracts              ->  "unit tests"
L3: Procedures                     ->  "implementation"
L4: Input Specifications           ->  "dependencies"
L5: Sourcing Requirements          ->  "infrastructure"
```

### Level 0: Customer Experience Contracts

What customers should perceive and feel. Three contract types:

- **Experience**: Customer-derived goals (e.g., "espresso is balanced, sweet, clean finish")
- **Constraint**: Regulatory requirements (e.g., EU Regulation 852/2004 food safety)
- **Commitment**: Voluntary organizational promises (e.g., "all coffee from direct trade")

These are the acceptance tests. Everything below exists to satisfy them.

### Level 1: Signal Requirements

What signals must the business emit to produce desired experiences? Mapped across Spectral Brand Theory's eight perception dimensions: semiotic, narrative, ideological, experiential, social, economic, cultural, temporal.

Each signal links to one or more L0 contracts via `satisfies_experience`.

### Level 2: Process Contracts

What each process must achieve -- measurable outcomes with quality gates, not methods. These are **executor-invariant**: the contract does not change when you swap a human barista for an automated machine.

Each contract links to L1 signals via `satisfies_signal`.

### Level 3: Procedures

How each process contract is executed -- step-by-step, executor-specific. This is the only level that changes when you change the executor type (human, hybrid, automated).

Each procedure links to an L2 contract via `implements_contract`.

### Level 4: Input Specifications

Materials, equipment, and capabilities each procedure requires. Measurable specifications with costs.

Each input links to an L3 procedure via `requires_input`.

### Level 5: Sourcing Requirements

Where inputs come from -- supplier criteria, logistics, contingency planning.

Each sourcing requirement links to an L4 input via `sourced_from`.

## Key Principles

### Contract-Procedure Separation

Contracts (tests) do not change when you swap the executor (implementation). Approximately 75% of a complete specification is executor-invariant -- defined at Levels 0-2 and shared across all executor configurations.

### Forkability

Sharing a business means sharing its test suite. A new owner copies the contracts (L0-L2), rewrites the procedures (L3-L5) for their context, and validates that the new implementation still passes the tests. This is how software teams fork open-source projects.

### Schema/Data Distinction

The **schema** (what parameters exist and what to measure) is publishable. The **data** (specific parameter values) is the competitive moat. Toyota published the Toyota Production System methodology; they kept their specific parameters.

### Backward Traceability

Every operational parameter traces backward through the cascade to the customer experience goal it serves. If a parameter cannot trace to an L0 contract, it is either waste (muda) or a missing contract.

### CI/CD as Satisfaction Validation

A validation pipeline checks: schema correctness, cross-reference integrity, contract satisfaction, signal coverage, experience traceability, and waste detection. This is the test runner for the business specification.

## Relationship to Spectral Brand Theory

Organizational Schema Theory and Spectral Brand Theory are sibling frameworks:

- **SBT** models how brands are perceived (observer-dependent, 8 dimensions)
- **Orgschema** uses SBT as the test specification language for L0-L1

SBT provides the perception model. Orgschema provides the operational specification that produces the desired perception. Together, they connect brand strategy to business operations through a shared, testable interface.

## References

- Beck, K. (2003). *Test-Driven Development: By Example*. Addison-Wesley.
- Hevner, A.R. et al. (2004). Design Science in Information Systems Research. *MIS Quarterly*, 28(1), 75-105.
- Liker, J.K. (2004). *The Toyota Way*. McGraw-Hill.
- Ohno, T. (1988). *Toyota Production System*. Productivity Press.
- Zharnikov, D. (2026a). Spectral Brand Theory. [DOI: 10.5281/zenodo.18945912](https://doi.org/10.5281/zenodo.18945912).
- Zharnikov, D. (2026b). The Organizational Schema Theory. [DOI: 10.5281/zenodo.18946043](https://doi.org/10.5281/zenodo.18946043).
