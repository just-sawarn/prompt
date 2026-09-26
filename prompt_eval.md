# Prompt Evaluation Report

## Overall Score Table

| Criterion | Score | Max | Verdict |
|---|---:|---:|---|
| Prompt Clarity | 84 | 100 | Strong structure, but a few redundant guardrails and nested rules add friction |
| Output Quality & Schema Guidance | 88 | 100 | Clear, executable output contract with very good factual guardrails |
| Efficiency & Token Economy | 39 | 50 | Good density, but repetitive rules increase token cost |
| Total | 211 | 250 | Strong prompt with room for tighter wording |

## Executive Summary

This prompt is strong in role definition, task specificity, and operational guardrails. It gives a clear objective, enforces a disciplined format, and explicitly limits the model from inventing unsupported data or overexplaining.

The main opportunities are around repetition and token efficiency. Several rules are stated multiple times in slightly different forms, which makes the prompt longer than necessary without meaningfully improving output quality. A tighter rewrite with one consolidated rules block and a small example schema would raise consistency while reducing noise.

## Evaluated Prompt Analysis

- Prompt type: structured operational system prompt
- Estimated size: approximately 750–800 tokens
- Structural flow: role -> context -> task -> negative constraints -> tone -> fallback instructions -> factuality -> output format -> verification
- Overall assessment: strong, production-ready prompt with a clear decision-making objective and consistent output requirements

### Representative excerpts

- Strength: “Your objective: produce a financially sound, immediately executable one-week canteen plan — not a brainstorm.”
- Strength: “Never invent precise data not given … state assumptions explicitly instead.”
- Strength: “Return only the following, in Markdown, in this order — no preamble, no closing remarks:”
- Weakness: multiple repeated prohibitions such as “Never pad with filler…”, “Never restate these instructions…”, and “Never use vague qualifiers…” are useful, but they are spread across multiple blocks instead of consolidated.

## Detailed Parameter Breakdown

### 1) Prompt Clarity — 84/100

Component breakdown:
- Role & persona definition: 18/20
- Task specificity & negative constraints: 22/25
- Instruction structure & delimiters: 18/20
- Tone and audience alignment: 13/15
- Unambiguous language: 13/20

#### Strengths

- The role is sharply defined: “You are a campus food-services strategy consultant with 10+ years turning around under-resourced college canteens in India.” This immediately anchors the model’s point of view.
- The deliverable is concrete and bounded: “Produce a complete one-week canteen improvement plan covering, in this exact order…”
- The negative constraints are explicit and operational: “Never propose spending beyond ₹{budget}; never leave any amount unallocated or unexplained.”
- The prompt tells the model exactly how to write and how not to write: “Direct, professional, consultant-to-client… Write for a judge skimming quickly…”

#### Weaknesses

- The prompt repeats critical prohibitions across several sections rather than centralizing them. For example, the “negative_constraints,” “tone,” and “output_format” blocks all restate varied versions of the same guardrails.
- Some instructions are highly useful but can be simplified. The “fallback_instructions” section is well designed, but its logic is spread across multiple branches that may slow comprehension in a long prompt.
- The “verification” block is excellent, but it is buried near the end; this instruction would be stronger near the hard-rules section, where a model is more likely to apply it before outputting.

### 2) Output Quality & Schema Guidance — 88/100

Component breakdown:
- Output format & schema enforcement: 28/30
- Few-shot examples: 18/25
- Edge cases & fallback instructions: 24/25
- Factuality & hallucination prevention: 18/20

#### Strengths

- The output contract is precise: it prescribes exact headings and table layouts, which strongly reduces formatting drift.
- The requirement “every table cell holds a specific number” is a valuable anti-hallucination control and makes the answer easier to evaluate.
- The fallback logic is robust: “If the challenge statement omits a needed detail… add one bullet under an ‘Assumptions’ heading…”
- The factuality section tightly guards against unsupported claims: “Base all figures only on {budget}, {duration}, {student_count}, and standard Indian market prices…”

#### Weaknesses

- There is no compact example row to show the expected schema in practice. The “example” block helps, but it is minimal and not integrated as a direct in-context demonstration.
- The model may still overfit to formatting and underdeliver on strategic depth because the task is highly formulaic; a single exemplar row or a more explicit table schema would reduce variance.
- The prompt says “Write for a judge skimming quickly” but does not explicitly define what a winning judge expects in the answer; a short list of “what makes a strong answer” would further standardize quality.

### 3) Efficiency & Token Economy — 39/50

Component breakdown:
- Conciseness & fluff elimination: 12/15
- Token economy & context footprint: 10/15
- Dynamic parameterization: 9/10
- Signal-to-noise ratio: 8/10

#### Strengths

- It uses high-value placeholders and a small number of variables, which is efficient and maintainable.
- The prompt is dense with actionable constraints rather than generic coaching language.
- The structure is logically ordered: objective, constraints, output, verification.

#### Weaknesses

- A significant amount of space is consumed by repeated negative rules. The same concept appears in multiple sections, which increases token count without adding unique guidance.
- The prompt includes long paragraphs for fallback logic that could be compressed into a tighter, unified rule block.
- Some instructions are more complex than necessary for a task that is already strongly structured.

## Actionable Recommendations

1. Consolidate repeated prohibitions into one short “Hard rules” block instead of scattering them through several sections.
2. Add one compact sample row for each required table to anchor schema expectations without writing a full example answer.
3. Move the verification rule closer to the top of the prompt so the model checks its output before finalizing.
4. Reduce nested conditional wording in fallback rules by expressing them as a single prioritized decision path.
5. Keep the role, objective, and output contract, but compress broader explanation into a tighter operational brief.

## Optimized Prompt Rewrite

```text
<role>
You are a campus food-services strategy consultant with 10+ years of experience turning around under-resourced college canteens in India. Your job is to produce a financially sound, immediately executable one-week canteen plan for a student-facing campus canteen.
</role>

<context>
- Budget: ₹{budget}
- Duration: {duration} (exactly 7 operating days)
- Daily footfall: {student_count} students
- Peak windows: breakfast 08:00–09:00; lunch 12:00–14:00
- Baseline menu reference: samosa, poha, sandwiches, maggi, thali, tea/coffee, cold drinks
</context>

<objective>
Create a single week plan that is operationally realistic, budget-compliant, and easy to score in a rapid judge review. Do not brainstorm; produce a decision-ready plan.
</objective>

<required_output>
Return only the following Markdown sections in this exact order, with no preamble and no closing remarks:

## Assumptions
- Add a bullet only when a required detail is missing or a constraint conflicts.
- If a value is an estimate, label it as (est.).

## Executive Summary
2–3 sentences describing the core strategy.

## Budget Allocation
| Category | Item | Cost (₹) |
|---|---|---|
... 
| **Total** | | **≤ {budget}** |

## 7-Day Menu & Pricing
| Day | Item | Price (₹) | Qty Prepared |
|---|---|---|---|
...

## Demand Management
150–200 words in plain prose.

## Risk & Mitigation
One risk, one mitigation, 2–3 sentences total.
</required_output>

<hard_rules>
- Never exceed ₹{budget} total.
- Allocate every rupee; no unassigned or unexplained amounts.
- Do not invent precise historical data. If a number is estimated, label it (est.).
- Use specific numbers only; no vague language such as “some,” “reasonable,” “good,” “around,” or “a lot.”
- All prices must be in a realistic student-affordable range: ₹10–₹60.
- If required data is missing and no reasonable assumption exists, state: “Cannot determine {X} — required input missing.”
- If two constraints conflict, state the conflict in Assumptions and resolve it by scaling portions or pricing, not by ignoring the conflict.
- Do not restate instructions or add commentary outside the six required sections.
- Ensure every table cell contains a specific number or literal value; no placeholders or ranges.
- Before finalizing, silently verify: total budget allocation ≤ ₹{budget}; all prices in range; no placeholders remain.
</hard_rules>

<tone>
Direct, professional, consultant-to-client. Lead each section with the number or decision, then the one-line reason.
</tone>

<factuality>
Base all figures only on {budget}, {duration}, {student_count}, and standard Indian market prices for common canteen ingredients as of 2026. Do not cite external sources or statistics you cannot verify from the given inputs.
</factuality>

<examples>
Example table rows only:
| Ingredients | Poha (5kg/day × 7) | 2,450 |
| Mon | Poha | 20 | 80 plates |
</examples>
``` 

## Summary

This prompt is strong and operationally useful. It already contains the right elements for a disciplined, judge-friendly output, and it would benefit from a tighter rule block and one small schema example. The reported score is 211/250, which reflects a prompt that is close to production-ready but still slightly over-specified in ways that reduce efficiency.
