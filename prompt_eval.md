# Prompt Evaluation Report

## Overall Score

| Criterion | Score | Max |
|---|---:|---:|
| Prompt Clarity | 91 | 100 |
| Output Quality & Schema Guidance | 89 | 100 |
| Efficiency & Token Economy | 37 | 50 |
| Total | 217 | 250 |

## Executive Summary

This prompt is highly structured and operationally strong. It gives a clear persona, exact deliverables, hard constraints, and a strict final format, which makes it well suited for precise planning tasks in a constrained environment.

The main opportunity is reducing repetition and tightening the instruction density. A few sections repeat rules in different forms, and the prompt could benefit from a smaller, more explicit schema and a single concise decision hierarchy.

## Evaluated Prompt Analysis

- Prompt source: [prompt.md](prompt.md)
- Estimated token count: ~620–760 tokens (approximate)
- Structural overview: persona definition, context block, task requirements, negative constraints, tone guidance, fallback rules, factuality rules, output format contract, and verification checklist
- Overall assessment: strong prompt for production-style output, but slightly verbose and partially redundant in the rule layer

## Detailed Parameter Breakdown

### 1) Prompt Clarity — 91 / 100

| Sub-factor | Score | Notes |
|---|---:|---|
| Role & Persona Definition | 19/20 | Strong persona and objective are explicit and credible. |
| Task Specificity & Negative Constraints | 24/25 | The task sequence and non-negotiable rules are very clear. |
| Instruction Structure & Delimiters | 18/20 | Good use of labeled blocks and explicit ordering. |
| Tone, Style & Target Audience | 14/15 | Audience and consultant voice are clearly defined. |
| Unambiguous Language | 16/20 | Mostly direct, but some constraints are repeated across several sections. |

#### Strengths

- The prompt establishes a sharp role immediately: "You are a campus food-services strategy consultant with 10+ years turning around under-resourced college canteens in India."
- The task is explicit and bounded: "Produce a complete one-week canteen improvement plan covering, in this exact order: ..."
- Hard constraints are cleanly enforced: "Never propose spending beyond ₹{budget}; never leave any amount unallocated or unexplained"
- The prompt defines the reader and the output style: "Write for a judge skimming quickly, not a general reader — lead every section with the number or decision, then the one-line reason."

#### Weaknesses

- The same constraint theme is repeated across several blocks (budget, no guessing, no filler, no commentary, exact output order), which raises instruction load without adding new guidance.
- The instruction set is long enough that a model may prioritize the most salient rules but not all of the repeated edge-case logic in the same way.
- Some fallback logic is correct but slightly over-specified for a single prompt; it could be simplified into a single assumption rubric.

### 2) Output Quality & Schema Guidance — 89 / 100

| Sub-factor | Score | Notes |
|---|---:|---|
| Output Format & Schema Enforcement | 29/30 | Strong contract with exact order and table shapes. |
| Few-Shot Examples | 18/25 | One example is provided, but more examples would improve consistency. |
| Edge Cases & Fallback Instructions | 23/25 | Good assumptions and conflict-handling instructions. |
| Factuality & Hallucination Prevention | 19/20 | Excellent guardrails against invented numbers and unsupported claims. |

#### Strengths

- Output contract is excellent: "Return only the following, in Markdown, in this order — no preamble, no closing remarks:"
- Verification instructions are strong: "Before finalizing, silently check: (1) Budget Allocation sums to ≤ ₹{budget}, ..."
- Factuality is explicitly controlled: "Do not cite external sources, studies, or statistics you cannot verify from the given inputs."
- The prompt gives concrete grammar and formatting expectations for both prose and tables, which is highly useful.

#### Weaknesses

- There is no explicit example of a fully valid completions set beyond a few rows, which could improve the model's formatting reliability.
- Some sections are still open to interpretation because the requested table content is a bit broad and could produce different valid stylings for the same business logic.
- The prompt does not clearly define a decision hierarchy for when assumptions are needed versus when a section must say "Cannot determine {X} — required input missing"; this could be sharper.

### 3) Efficiency & Token Economy — 37 / 50

| Sub-factor | Score | Notes |
|---|---:|---|
| Conciseness & Fluff Elimination | 12/15 | Mostly concise, but redundant constraints and repeated admonitions add length. |
| Token Economy & Context Footprint | 11/15 | Dense and useful, but larger than necessary for the objective. |
| Dynamic Parameterization | 7/10 | Placeholders are clear, but the repeated use of {budget}, {duration}, and {student_count} could be simplified into a single input block. |
| Signal-to-Noise Ratio | 7/10 | High-value rules are present, but some repeated statements weaken the signal. |

#### Strengths

- The prompt removes filler effectively: "Never pad with filler (...) — start directly with content"
- Every major directive is tied to the actual output contract, which keeps most instructions task-relevant.
- The prompt compresses business logic and output constraints into a single defined job, which is efficient for a planning task.

#### Weaknesses

- There is repetition across the sections: negative constraints, factuality, tone, verification, and output format each reassert the same intent in slightly different wording.
- The numbered requirement list plus the output contract plus the verification checklist create a heavier instruction stack than necessary.
- Some lines are more like meta-policy than model behavior, which slightly increases context overhead without changing the final output quality.

## Actionable Recommendations

1. Consolidate overlapping rules into a single "Hard Rules" section and remove repeated admonitions across later blocks.
2. Add a compact example output table demonstrating one valid row and one valid section-style response to reduce formatting drift.
3. Define a single assumption policy in one sentence: "When a value is missing, use the minimum justified assumption and label it clearly as an assumption."
4. Reduce meta-commentary and keep the verification checklist limited to the exact checks that materially affect correctness.
5. Streamline the prompt by consolidating context, task, and output structure into a clearer XML-like block layout such as `<role>`, `<inputs>`, `<required_output>`, and `<rules>`.

## Optimized Prompt Rewrite

```text
<role>
You are a campus food-services strategy consultant with 10+ years of experience improving under-resourced college canteens in India. Your job is to produce a financially sound, immediately executable one-week canteen plan for a student canteen operating in a constrained-budget environment.
</role>

<inputs>
- Budget: ₹{budget}
- Duration: {duration} (must be exactly 7 operating days)
- Daily footfall: {student_count} students
- Peak windows: breakfast 08:00–09:00 and lunch 12:00–14:00
- Baseline menu reference: standard Indian college canteen items (samosa, poha, sandwiches, maggi, thali, tea/coffee, cold drinks)
</inputs>

<task>
Create a complete one-week canteen improvement plan using the exact structure below, in this order:
1. Menu decisions — kept/added/dropped items with justification by cost, popularity, and prep complexity
2. Budget allocation — every rupee of ₹{budget} assigned to a category
3. Pricing strategy — per-item price and the pricing logic used
4. Quantity and inventory plan — daily prep quantities sized against stockout and wastage risk
5. Demand management — actions to handle peak-hour load and smooth week-long demand
6. Risk and contingency — one realistic failure mode and one mitigation
</task>

<hard_rules>
- Do not exceed ₹{budget}; every rupee must be allocated or explicitly explained.
- Do not invent precise historical data; use assumptions only when required and label them clearly.
- Use specific numeric values for every price, cost, and quantity. No ranges, placeholders, or vague phrases.
- Do not add commentary outside the six required sections.
- Do not use filler phrases such as "Great question," "I would be happy to," or "In conclusion."
- Start each section directly with the result or decision.
- Base all figures only on the provided inputs and standard 2026 Indian market pricing for common canteen ingredients.
- If an input is missing or conflicting, state the single most realistic assumption under an "Assumptions" heading; if no reasonable assumption exists, state: "Cannot determine {X} — required input missing."
</hard_rules>

<tone>
Direct, professional, consultant-to-client. Use concise, executive language for a judge who will skim the output in about 2 minutes.
</tone>

<required_output>
Return only the following in Markdown, in this exact order:

## Assumptions
- Bullet list only if assumptions were needed.

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

<verification>
Before finalizing, silently verify:
1. Total budget allocation is ≤ ₹{budget}
2. Every price is realistic for student affordability and falls in the ₹10–60 range
3. Every table cell contains a specific numeric value, never a placeholder or range
4. The structure matches the required ordering exactly
</verification>
</content>
