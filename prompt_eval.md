# Prompt Evaluation Report

## Overall Score Table

| Criterion | Score | Max Score |
|---|---:|---:|
| Prompt Clarity | 90 | 100 |
| Output Quality & Schema Guidance | 89 | 100 |
| Efficiency & Token Economy | 42 | 50 |
| Total | 221 | 250 |

## Executive Summary

This prompt is strong in role definition, task sequencing, and operating guardrails. It gives a highly bounded consultant task with explicit output structure, negative constraints, and a verification checklist, which makes it likely to produce decision-ready and judge-friendly responses.

The main opportunity is reducing repetition across sections and adding a minimal example to make the output format even more deterministic. The prompt already has a strong base; it mostly needs tightening rather than a complete rewrite.

## Evaluated Prompt Analysis

- Source: [prompt.md](prompt.md)
- Estimated prompt length: approximately 650–700 tokens
- Structural overview: role definition, context, task, negative constraints, tone, fallback logic, factuality rules, output format specification, example, and final verification checklist
- Strength: clear persona and operational objective anchored to a specific business problem
- Strength: very explicit output order and exact formatting requirements
- Improvement area: repeated rules across multiple blocks increase token count without adding much new guidance

## Detailed Parameter Breakdown

### 1) Prompt Clarity — 90/100

Score breakdown:
- Role & persona definition: 18/20
- Task specificity & negative constraints: 23/25
- Instruction structure & delimiters: 18/20
- Tone/style/target audience: 15/15
- Unambiguous language: 16/20

Strengths:
- The opening sentence, "You are a campus food-services strategy consultant with 10+ years...", firmly anchors the model role and domain expertise.
- "Produce a complete one-week canteen improvement plan covering, in this exact order" creates strong sequencing clarity.
- The negative constraints are concrete and operational: "Never propose spending beyond ₹{budget}; never leave any amount unallocated or unexplained" and "Never use vague qualifiers..." are highly specific.
- The fallback section guides edge behavior clearly: assumptions, conflict resolution, and missing inputs are handled in a deterministic way.

Weaknesses:
- Some rules repeat across sections: budget limits, exact numbers, no filler language, and no commentary outside the required sections appear more than once.
- The prompt is precise, but a few lines could be condensed without losing meaning.
- The assumptions rule is generally good, but it could be more explicit about the preferred scope of the assumption (for example: only one single assumption, not a list of broad approximations).

### 2) Output Quality & Schema Guidance — 89/100

Score breakdown:
- Output format & schema enforcement: 28/30
- Few-shot examples & in-context demonstrations: 19/25
- Edge cases & fallback instructions: 23/25
- Factuality & hallucination prevention: 19/20

Strengths:
- The required output structure is meticulous and highly actionable. The exact order and markdown requirements reduce model uncertainty.
- The verification clause, "Before finalizing, silently check: (1) Budget Allocation sums to ≤ ₹{budget}, (2) every price falls in a realistic ₹10–60 student-affordable range..." is an excellent quality-control device.
- The example section demonstrates the intended table format and encourages cell-level specificity.
- The factuality section is strong: it instructs the model to label estimates as "(est.)" and to avoid unsupported statistics.

Weaknesses:
- The example is only a partial row, not a complete mini-sample output. A single full example could improve format adherence further.
- The prompt handles missing data well, but it does not offer a strong explicit rule for contradictory inputs beyond a single sentence under assumptions.
- A more concrete example of a refusal/repair pattern could improve consistency when required inputs are absent.

### 3) Efficiency & Token Economy — 42/50

Score breakdown:
- Conciseness & fluff elimination: 13/15
- Token economy & context footprint: 13/15
- Dynamic parameterization: 9/10
- Signal-to-noise ratio: 7/10

Strengths:
- The prompt is dense and action-oriented rather than conversational.
- Variable injection points like {budget}, {duration}, and {student_count} are clearly marked and easy to substitute.
- The core instructions are prioritized well: role, objective, task, output format, verification.

Weaknesses:
- Repeated rules across multiple sections add unnecessary token count.
- The final verification block is valuable but slightly heavy for a prompt meant to be reused across generations.
- Some phrasing could be more compact without lowering clarity, especially in the "never" and "if ... then" clauses.

## Actionable Recommendations

1. Merge repeated constraints into a single concise rules block to reduce redundancy.
2. Add one complete mini input/output example instead of a partial row to improve output determinism.
3. Clarify the conflict-resolution behavior for contradictory inputs so the model follows a single default approach.
4. Trim repeated wording in the verification and negative-constraint sections to maintain a higher signal-to-noise ratio.
5. Keep the assumptions rule, but limit it to one realistic assumption or one sentence of conflict flagging whenever possible.

## Optimized Prompt Rewrite (Production-Ready)

```text
<role>
You are a campus food-services strategy consultant with 10+ years of experience turning around under-resourced college canteens in India. Your specialty is constrained-budget operations: menu engineering, cost-plus pricing, demand forecasting, and queue management for 300–500 daily-footfall environments.
</role>

<context>
- Budget: ₹{budget} — one-time seed capital for the week, not a recurring subsidy
- Duration: {duration} (exactly 7 operating days)
- Daily footfall: {student_count} students (typical range 300–500)
- Peak windows: breakfast 08:00–09:00, lunch 12:00–14:00
- Baseline menu reference: standard Indian college-canteen items (samosa, poha, sandwiches, maggi, thali, tea/coffee, cold drinks)
- Audience: student-competition judges scoring against a rubric in about 2 minutes each; output must be scannable and immediately actionable
</context>

<objective>
Produce a financially sound, immediately executable one-week canteen improvement plan. Prioritize operational feasibility and budget discipline over broad ideas.
</objective>

<required_output>
Return only the following Markdown sections, in this exact order; no preamble and no closing remarks:

## Assumptions
- Include only if a needed detail is missing or two constraints conflict.
- If no assumption is required, omit this section entirely.

## Executive Summary
2–3 sentences stating the core strategy.

## Budget Allocation
| Category | Item | Cost (₹) |
|---|---|---|
...
| **Total** | | **≤ ₹{budget}** |

## 7-Day Menu & Pricing
| Day | Item | Price (₹) | Qty Prepared |
|---|---|---|---|
...

## Demand Management
150–200 words, plain prose.

## Risk & Mitigation
One risk, one mitigation, 2–3 sentences total.
</required_output>

<rules>
- Never spend beyond ₹{budget}; every rupee must be allocated or clearly explained.
- Never leave any amount unallocated or unexplained.
- Never invent precise data not provided; if a number is an estimate, label it "(est.)".
- Never use vague qualifiers such as "some items," "a good number," or "reasonable price." Every quantity, price, and cost must be a specific number.
- Never add filler, commentary, or restatements of the instructions outside the six required sections.
- Use direct, professional consultant-to-client language; do not hedge with "might," "could potentially," or similar phrasing.
- Start each section with the number or decision, then the one-line reason.
- All figures must be based only on {budget}, {duration}, {student_count}, and standard Indian market prices for common canteen ingredients as of 2026.
- If a required input is missing and no reasonable assumption exists, state "Cannot determine {X} — required input missing" in that section instead of guessing.
- If two constraints conflict, state the conflict in one sentence under Assumptions and resolve it by scaling portions or pricing, not by ignoring the issue.
</rules>

<format_requirements>
- Use only Markdown tables and plain prose.
- Every table cell must contain a specific number or category label; no placeholders or ranges.
- Every item price must fall within a realistic ₹10–60 student-affordable range.
- Every row must be specific and auditable.
- The final answer must be scannable for judges reading in under 2 minutes.
</format_requirements>

<quality_checks>
Before finalizing, silently verify:
1. Budget Allocation totals ≤ ₹{budget}.
2. Every price is in a realistic ₹10–60 student-affordable range.
3. Every table cell contains a specific number, not a placeholder or range.
4. All assumptions are minimal, explicit, and necessary.
</quality_checks>

<example>
Correctly formatted rows only:
| Ingredients | Poha (5kg/day × 7) | 2,450 |
| Mon | Poha | 20 | 80 plates |
</example>
```

---

This prompt is already well-structured and would perform strongly with minimal adjustment. The main gains come from reducing repetition and adding one compact example, which improves both reliability and token efficiency without diminishing the operational detail level.
