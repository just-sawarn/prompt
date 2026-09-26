# Prompt Evaluation Report

## Overall Score

| Parameter | Score | Maximum |
|---|---:|---:|
| Prompt Clarity | 85 | 100 |
| Output Quality & Schema Compliance | 51 | 100 |
| Efficiency & Token Economy | 34 | 50 |
| **Total** | **170** | **250** |

## Executive Summary

The prompt gives a concrete Indian college-canteen scenario, a fixed one-week horizon, a defined seed budget, service-volume and peak-hour context, and a broad set of operational decisions to make. Its numbered task list and requested tables encourage useful coverage. The main weaknesses are missing financial and operating assumptions, limited controls against invented cost estimates, and no worked example to show the expected arithmetic and level of detail. The request to “think through the trade-offs step by step” should be replaced with a request for concise, checkable rationale.

## Evaluated Prompt Analysis

**Target:** `prompt.md`
**Estimated length:** approximately 400 tokens (rough estimate; tokenizer-dependent).
**Structure:** Role and scenario; six required planning areas; a five-part output specification; three hard constraints; final calculation/reasoning instruction.

The prompt is intended to elicit a practical seven-day plan for a canteen serving roughly 300–500 students daily, with ₹10,000 of one-time seed capital. It asks for menu selection, allocation, pricing, daily quantities, demand handling, and contingency planning. Its central deliverable is strongly specified by section, but the calculations depend on local prices, current equipment and staff capacity, and the interpretation of how sales revenue may be used during the week. Those inputs are not provided or assigned an explicit assumption policy.

## Detailed Parameter Breakdown

### 1. Prompt Clarity: 85 / 100

| Criterion | Score | Evidence and rationale |
|---|---:|---|
| Role & Persona Definition | 18 / 20 | “Experienced campus food-services consultant” and “turned around underperforming college canteens on tight budgets” establish relevant perspective and objective. Experience is asserted rather than operationally bounded, but the role is apt. |
| Task Specificity & Negative Constraints | 22 / 25 | The six enumerated areas are concrete, and “not as a subsidy to give food away” is a useful boundary. The prompt does not specify whether the canteen's existing staff, equipment, suppliers, or starting inventory are available. |
| Instruction Structure & Delimiters | 17 / 20 | `CONTEXT`, `TASK`, `OUTPUT FORMAT`, and `CONSTRAINTS` separate the major instructions cleanly. There are no explicit data slots or separation mechanism for assumptions versus user-provided facts. |
| Tone, Style & Target Audience | 12 / 15 | Student affordability and the Indian college context imply the audience and practical tone. It does not expressly define response style, technical level, or whether estimates should be labeled. |
| Unambiguous Language | 16 / 20 | “Exactly one week (7 days)” and the service periods are specific. “Every rupee must be accounted for” sits in tension with “must not exceed ₹10,000,” and “typical Indian college canteen menu” has no defined baseline. |
| **Subtotal** | **85 / 100** | |

**Strengths**

- “The canteen serves roughly 300–500 students daily, with peak rushes at breakfast (8–9 AM) and lunch (12–2 PM)” gives planning-relevant demand and timing context.
- “The goal is to maximize student satisfaction and canteen revenue/sustainability within this single week” gives a clear, multi-objective purpose.
- The numbered task list makes it difficult to overlook major operating decisions.

**Weaknesses**

- “Typical Indian college canteen menu” leaves the baseline menu unspecified, so menu keep/drop decisions cannot be tied to known current sales or facilities.
- “Every rupee must be accounted for” may be read as requiring exactly ₹10,000 expenditure, despite the separate upper-bound constraint.
- Staff, equipment, storage, existing inventory, payment/ordering methods, and permission for sales receipts to fund replenishment are unspecified.

### 2. Output Quality & Schema Compliance: 51 / 100

| Criterion | Score | Evidence and rationale |
|---|---:|---|
| Output Format & Schema Enforcement | 23 / 30 | The prompt requires an executive summary, budget table, seven-day menu/pricing table, 150–200-word demand section, and one key risk. It does not prescribe fields for quantities, daily budget reconciliation, unit costs, daily totals, or assumptions. |
| Few-Shot Examples & Demonstrations | 0 / 25 | No example input/output or sample calculation is provided. |
| Edge Cases & Fallback Instructions | 18 / 25 | It names a flop, raw-material price spike, and bad-weather footfall drop, and asks for stockout and overproduction controls. It does not explain what to do when costs or local facts are unavailable, or how to handle contradictory budget arithmetic. |
| Factuality & Hallucination Prevention | 10 / 20 | “Show the math” supports auditability, but no instruction distinguishes supplied facts from estimates or prohibits presenting invented local prices as verified facts. |
| **Subtotal** | **51 / 100** | |

**Strengths**

- The output requirements constrain the answer to usable artifacts, especially a “day-wise menu + pricing table for the 7 days.”
- The plan must include both demand and inventory controls, not only a menu and spending proposal.
- Naming representative risks points the model toward operational contingencies.

**Weaknesses**

- The budget table schema (`item | cost | category`) cannot by itself show assumptions, quantity/unit cost, timing, or a reconciled total.
- No example demonstrates that quantities, revenue assumptions, or the budget should be internally consistent.
- The prompt does not direct the model to label estimates, state assumptions, or avoid claiming unsupported local market prices.
- Requiring one key risk underspecifies the three risk examples and could cause valid contingencies to be omitted.

### 3. Efficiency & Token Economy: 34 / 50

| Criterion | Score | Evidence and rationale |
|---|---:|---|
| Conciseness & Fluff Elimination | 13 / 15 | The prompt is compact relative to the six requested workstreams. Some examples and repeated budget/constraint instructions could be consolidated. |
| Token Economy & Context Footprint | 13 / 15 | Most details are decision-relevant. Repeating the budget limit and asking for the budget sum in multiple places adds modest duplication. |
| Dynamic Parameterization | 0 / 10 | The single scenario is hard-coded; no reusable fields for location, budget, sales, existing menu, staffing, or operating hours are supplied. |
| Signal-to-Noise Ratio | 8 / 10 | The headings and numbered tasks prioritize requirements effectively. “Think through the trade-offs step by step” is not needed for an auditable final answer and may encourage excessive reasoning text. |
| **Subtotal** | **34 / 50** | |

## Actionable Recommendations

1. Define what the ₹10,000 covers and clarify whether sales revenue may finance later purchases. State whether existing staff, equipment, inventory, and facilities are available; otherwise require explicit assumptions.
2. Replace “every rupee must be accounted for” with a reconciliation rule: show planned allocations totaling no more than ₹10,000, identify any unspent reserve, and verify the sum.
3. Expand the table schema to include daily quantities, selling price, estimated unit cost, expected sales/revenue, and daily and weekly totals. Mark estimates as assumptions rather than verified local data.
4. Ask for concise decision rationale and visible calculations, not hidden step-by-step reasoning.
5. Require fallback behavior for unknown or missing local data, and give a compact example of a correctly reconciled budget row or total.
6. Cover each named risk briefly, or explicitly request one prioritized risk plus short mitigations for the others.
7. Parameterize the context if the prompt is intended for reuse across canteens; otherwise retain the fixed scenario and identify which values are estimates.

## Optimized Prompt Rewrite (Production-Ready)

```text
<role>
You are a practical campus food-services consultant. Create a low-cost, operationally realistic plan that improves student satisfaction and supports canteen revenue without giving food away.
</role>

<scenario>
- Location/context: Indian college canteen; use ₹ and student-budget pricing.
- Planning period: 7 consecutive days.
- One-time seed budget: maximum ₹10,000 for changes and operating inputs during this week.
- Demand: approximately 300–500 students per day; breakfast peak 8–9 AM; lunch peak 12–2 PM.
- Existing menu examples: samosa, poha, sandwiches, Maggi, thali, cold drinks. Treat these as examples, not confirmed current offerings.
- No funding beyond the stated seed budget. Do not assume revenue can fund later purchases unless you state that assumption; distinguish seed spending from any revenue-funded replenishment.
</scenario>

<task>
Design a complete seven-day improvement plan. Make operational recommendations for:
1. Menu: items to keep, add, or drop, with concise reasons based on affordability, likely demand, ingredient cost, and preparation complexity.
2. Budget: allocate the seed budget among ingredients, small equipment/utensils, signage, staff incentives, and contingency as appropriate.
3. Pricing: set realistic student-facing prices and explain the cost, affordability, and margin trade-offs; include any combo prices.
4. Inventory: give estimated daily preparation quantities per item and a simple reorder/stop-prep rule to limit stockouts and waste.
5. Demand: address both stated peak periods and slower hours/days using workable service or promotion changes.
6. Risks: name the most important risk and mitigation; briefly cover a weak-selling item, input-cost increase, and lower footfall if not already covered.
</task>

<assumptions_and_accuracy>
Do not present unknown local prices, sales, margins, or facilities as verified facts. If needed, state a short list of reasonable planning assumptions and label all estimates. If a required input is unavailable, proceed with a clearly labeled assumption rather than inventing a source. Show concise calculations and conclusions; do not provide private chain-of-thought.
</assumptions_and_accuracy>

<output_format>
Return the sections in this order:
1. Executive summary: 2–3 lines.
2. Assumptions: concise bullets, including starting equipment/staff and whether sales revenue can be reused.
3. Budget table with columns: item/use | quantity or basis | cost (₹) | category. Show the arithmetic and a total. Total planned seed spending must be ≤ ₹10,000; identify any unspent balance as reserve, not as spent money.
4. Seven-day menu and operations table with columns: day | item | prep quantity | estimated unit cost (₹) | selling price (₹) | brief rationale or service note. Include daily totals or a separate compact daily summary.
5. Pricing and inventory rules: concise explanation of price logic, replenishment, stockout response, and waste reduction.
6. Demand management: 150–200 words, covering the breakfast/lunch peaks and quieter periods.
7. Risks: one prioritized risk with mitigation, plus brief responses to the other named scenarios.
8. Validation: state the budget sum and confirm it does not exceed ₹10,000. Keep any sales/revenue projection separate from the seed-budget spending total.
</output_format>

<constraints>
- Do not exceed the ₹10,000 seed budget or assume additional funding.
- Keep prices plausible for an Indian student canteen and identify them as estimates where local data is unavailable.
- Ensure quantities, unit costs, totals, and prices are internally consistent. If exact precision is unsupported, use rounded estimates and disclose the basis.
- Keep the plan actionable for a one-week trial; avoid buying durable equipment unless its use during this week is justified.
</constraints>
```