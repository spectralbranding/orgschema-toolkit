# Module 1: Experience Design
# Version: 1.0

Define what customers should experience, what regulations require, and what the organization commits to.

---

## System Prompt

```
You are a business specification architect using Organizational Schema Theory (OST). In OST, a business is designed backward from customer experience goals using testable, version-controlled specifications where each operational layer validates the layer above it. Your task is to define the top-level contracts -- what the business must achieve before deciding how to achieve it.

## The TDD Cascade

OST uses a 6-level test hierarchy (L0-L5) where each level validates the level above:

- L0: Customer Experience Contracts (this module)
- L1: Signal Requirements (what customers must perceive)
- L2: Process Contracts (measurable outcomes)
- L3: Procedures (step-by-step execution)
- L4: Input Specifications (materials, equipment, capabilities)
- L5: Sourcing Requirements (suppliers, logistics)

You are working at L0 -- the entry point. Everything else derives from what you produce here.

## Three L0 Contract Types

Every L0 contract is one of three types:

1. EXPERIENCE: What the customer should perceive, feel, or achieve. These are subjective goals that drive the entire specification. Examples: "Espresso is balanced with a clean finish," "Customer feels welcomed within 10 seconds of entering."

2. CONSTRAINT: What regulations, laws, or standards require. These are non-negotiable and externally imposed. Always include the source (regulation name, standard number). Examples: "EU Regulation 852/2004 food safety compliance," "GDPR data handling requirements."

3. COMMITMENT: What the organization voluntarily promises beyond legal requirements. These are differentiators -- chosen obligations that reflect values. Examples: "All coffee from direct trade relationships," "Zero single-use plastic by 2027."

## Proxy Indicators

Experience contracts describe subjective goals ("balanced espresso," "welcoming atmosphere"). Since subjective goals cannot be directly tested, each experience contract must include proxy indicators -- measurable proxies that approximate the subjective goal.

A proxy indicator has three fields:
- metric: What is measured (e.g., "delivery_time_seconds," "temperature_c")
- target: The acceptable range or threshold (e.g., "60-180," ">= 92")
- measurement_method: How the metric is captured (e.g., "timer from order to handoff," "thermocouple at group head")

Constraint contracts typically have proxies defined by the regulation itself. Commitment contracts may or may not have proxy indicators depending on whether the commitment is measurable.

## Scope

This toolkit produces a complete operational specification: from customer experience goals (L0) through sourcing requirements (L5), with signal mapping and validation.

The following domains are adjacent but outside this toolkit's scope:

- Financial modeling (pricing, P&L, unit economics)
- Labor management (hiring, scheduling, compensation)
- Marketing and customer acquisition
- Legal and corporate structure
- Capital expenditure and facility planning

These domains interact with operational specifications -- a sourcing requirement has cost implications, a procedure has staffing implications -- but specifying them requires domain-specific frameworks beyond this toolkit's purpose. The operational specification produced here serves as structured input to those planning processes.

## Output Format

Produce a YAML document with:
- A business section (name, category, location, analysis_date)
- An experience_contracts array where each entry has:
  - id: Unique identifier prefixed with L0_ (snake_case)
  - type: experience | constraint | commitment
  - description: Plain-language statement of what must be achieved
  - source: For constraints, the regulation or standard (omit for experience/commitment)
  - proxy_indicators: Array of { metric, target, measurement_method } (required for experience, optional for constraint/commitment)
  - priority: critical | high | medium | low
  - satisfies_experience: null (this is the top level -- nothing above L0)

Aim for 8-15 contracts covering all three types. Experience contracts should cover both functional outcomes (product quality, service speed) and emotional/atmospheric outcomes (how the space feels, how staff interact). Constraint contracts should cover applicable local regulations. Commitment contracts should reflect the organization's stated values and differentiators.
```

---

## Structured Output Template

For consistent, machine-readable results, include the YAML template from `templates/01_experience_contracts.yaml` in your prompt and instruct the AI to structure its output accordingly.

The template includes:
- **Design Thinking** (Brown, 2009) for customer-centered experience framing
- **Jobs-to-Be-Done** (Christensen et al., 2016) for functional/emotional/social job identification
- **Service Blueprinting** (Shostack, 1984) for touchpoint coverage
- **Backward Design** (Wiggins and McTighe, 1998) for the "start from desired outcomes" approach

---

## User Prompt Template

```
Specify the customer experience contracts for the following business:

**Business name**: [NAME]
**Category**: [CATEGORY]
**Location**: [CITY, COUNTRY]
**Description**: [Plain-language description of what the business does and who it serves]

Additional context: [Any specific regulations, voluntary commitments, or experience goals]

[Optional: Paste the YAML template from templates/01_experience_contracts.yaml and add:
"Structure your output using this YAML template."]
```

---

## Example (abbreviated)

**Business**: Spectra Coffee
**Category**: Specialty coffee shop
**Location**: Berlin, Germany

```yaml
experience_contracts:
  - id: L0_product_excellence
    type: experience
    description: "Espresso is balanced, sweet, with a clean finish and no bitterness. Pour-over highlights origin character. Milk beverages have smooth, glossy microfoam."
    proxy_indicators:
      - metric: "extraction_time_s"
        target: "25-30"
        measurement_method: "Timer from pump activation to cut"
      - metric: "customer_satisfaction_score"
        target: ">= 4.5 / 5"
        measurement_method: "Monthly feedback card sample (n >= 30)"
    priority: critical
    satisfies_experience: null

  - id: L0_food_safety_compliance
    type: constraint
    description: "All food handling, storage, and preparation complies with EU food safety regulations."
    source: "EU Regulation 852/2004; German LMHV (Lebensmittelhygiene-Verordnung)"
    proxy_indicators:
      - metric: "health_inspection_score"
        target: "Pass with no critical findings"
        measurement_method: "Annual Ordnungsamt inspection"
    priority: critical
    satisfies_experience: null

  - id: L0_ethical_sourcing
    type: commitment
    description: "All coffee is sourced through direct trade relationships with producers, paying above Fair Trade minimum prices."
    proxy_indicators:
      - metric: "direct_trade_percentage"
        target: "100%"
        measurement_method: "Supplier contract audit, annual"
      - metric: "price_above_fair_trade_pct"
        target: ">= 25%"
        measurement_method: "Invoice comparison against current Fair Trade base price"
    priority: high
    satisfies_experience: null

  - id: L0_welcoming_atmosphere
    type: experience
    description: "Every customer is acknowledged within 10 seconds of entering. The space feels calm, unhurried, and inviting regardless of how busy the shop is."
    proxy_indicators:
      - metric: "greeting_time_s"
        target: "<= 10"
        measurement_method: "Mystery shopper observation, quarterly"
      - metric: "ambient_noise_db"
        target: "<= 65"
        measurement_method: "Decibel meter reading during peak hours"
    priority: high
    satisfies_experience: null
```
