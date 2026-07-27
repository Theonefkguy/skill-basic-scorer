---
name: skill-basic-scorer
description: Manually score and audit Codex skills with a custom, modern prompt-fit 100-point rubric. Use when the user asks to rate, score, compare, rank, audit, or review one or more Skill folders or SKILL.md files; when they want before/after skill-quality comparisons; or when they need evidence-backed improvements to metadata, scope, outcomes, prompt economy, evidence handling, output contracts, validation, or safety. This skill uses human-readable rubric judgment rather than an automatic keyword scorer.
---

# Skill Basic Scorer

## Overview

Use this skill to produce an evidence-based manual score for Codex skills. Treat the score as a static quality estimate, not proof of live task performance.

Score for modern prompt fitness, not instruction volume. Strong skills define the outcome, constraints, available evidence, output shape, and stop conditions while leaving Codex enough room to choose an efficient solution path.

This rubric is an author-defined review heuristic, not an OpenAI certification standard. Its dimension weights and grade thresholds are useful for consistent comparison but are not official OpenAI scores.

Static review should stay low-risk: inspect skill text, metadata, bundled-resource references, and script affordances. Do not execute arbitrary scripts bundled inside the target skill unless the user explicitly asks for deeper validation.

## Workflow

1. Identify the target skill folder, `SKILL.md`, or skills root. If a root is provided, score direct child folders that contain `SKILL.md`.
2. Read [references/basic-rubric.md](references/basic-rubric.md) completely before assigning scores.
3. Read the target `SKILL.md`, `agents/openai.yaml` when present, and each resource needed to verify the skill's claims. Check whether essential resources are linked with clear when-to-load guidance.
4. Optionally run the bundled `skill-creator` structural validator when available. Resolve the installed `skill-creator` directory from the current skills catalog instead of assuming a user-specific path:

```bash
python3 <skill-creator-dir>/scripts/quick_validate.py <skill-folder>
```

   Treat validator output as structural evidence only; it does not determine the numeric quality score. If the validator or one of its dependencies is unavailable, report that limitation and continue the manual review.
5. Score every rubric dimension manually. For each dimension, record the strongest positive evidence, the concrete penalty, and the reason for the awarded points. Do not infer quality from keyword counts or file length alone.
6. Inspect the highest-impact findings before finalizing:
   - Does the skill say what good work looks like?
   - Are required constraints and evidence boundaries clear?
   - Does it avoid carrying excess legacy process into the prompt?
   - Are output expectations, missing-context behavior, and stop rules explicit enough?
   - Are bundled resources linked from `SKILL.md` with clear when-to-load guidance?
   - Are optional frontmatter fields spec-aligned rather than stale or invented?
7. For before/after comparisons, use the same rubric, assumptions, and inspected resource set for both versions. Explain tradeoffs, not only the point change.
8. Assign confidence separately from grade. Lower confidence when essential resources are inaccessible, behavior depends on untested scripts/backends, or representative task evidence is absent.
9. When the user asks whether a skill follows current OpenAI guidance, compare it with current authoritative sources. Separate:
   - Agent Skills specification requirements
   - OpenAI product guidance and recommendations
   - This rubric's custom heuristics
   - Live benchmark evidence, if any

## Scoring Model

Use a 100-point rubric:

- Discoverability and metadata: 12
- Scope and trigger fit: 10
- Outcome and success criteria: 14
- Workflow judgment and degrees of freedom: 12
- Prompt economy and progressive disclosure: 12
- Evidence, retrieval, and missing-context behavior: 10
- Output contract and stop rules: 12
- Validation and benchmark guidance: 9
- Safety and operational guardrails: 9

These weights and grade bands are custom calibration choices, not OpenAI-defined ratings.

Mark a dimension `N/A` when it is genuinely irrelevant to the skill's domain and its omission does not create a functional or operational gap. Calculate the normalized total as:

```text
earned points / maximum applicable points × 100
```

Do not add boilerplate merely to make a dimension applicable. Grades use the normalized total: A = 90-100, B = 80-89, C = 70-79, D = 60-69, F = below 60.

## Output Contract

For a single skill, report:

- Total score and grade
- Confidence: high, medium, or low
- Dimension breakdown
- Important positive evidence and penalties for the weakest dimensions
- Resource health: broken links, unmentioned resources, harmful nesting, or script static-health concerns
- 3-5 evidence-backed findings
- Top 1-3 low-risk improvement suggestions tied to concrete penalties, not generic dimension names
- Any validation failure or caveat
- Whether each conclusion comes from the Agent Skills specification, current OpenAI guidance, custom rubric judgment, validator output, or live benchmark evidence

For multiple skills, include a summary table sorted by lowest score or largest regression risk, then highlight the best upgrade candidates, non-high-confidence results, and broken-resource clusters.

For modern Codex prompt-fit reviews, favor concise findings over long process commentary. Explain the most important tradeoff when a skill gains clarity by being shorter or loses flexibility by over-specifying the route.

Stop once the user has enough information to act on the static score: the score, confidence, strongest evidence, highest-risk penalties, and the fewest useful improvements. Do not continue searching for marginal issues after the main static findings are clear.

Completion means the final response clearly separates validator results, manual judgment, static-review caveats, and the next low-risk improvement actions.

## Guardrails

- Do not rewrite a skill unless the user asks for changes.
- Do not use an automatic keyword or length-based scoring heuristic. Assign every point through rubric-based judgment and cited evidence.
- Do not score solely by length. A short redirect shim can be good if its purpose is explicit and safe.
- Do not reward step count for its own sake. Over-specified workflows can reduce score when judgment rules or outcomes would guide better.
- Treat `always`, `never`, `must`, and `only` as appropriate for true invariants, not as automatic quality markers.
- Penalize fake paths, fake links, stale skill names, unsupported frontmatter, broken resource links, and unverifiable claims.
- Treat optional frontmatter fields (`license`, `compatibility`, `metadata`, `allowed-tools`) as valid when they follow the Agent Skills spec. Note that `allowed-tools` is experimental and support can vary by agent implementation. Penalize invented top-level fields, stale names, and overlong descriptions.
- Penalize generic keyword stuffing: many vague quality phrases, long descriptions with little concrete scope, or rigid step lists that lack output/stop criteria.
- Do not execute target skill scripts during baseline static review. Inspect whether dependencies, inputs, outputs, and failure behavior are documented appropriately, then caveat uncertainty.
- Reward clear trigger descriptions, outcome-first success criteria, concise workflows, explicit output expectations, progressive references, validation steps, evidence budgets, and safety boundaries.
- Apply evidence, retrieval, validation, and safety expectations in proportion to the domain. Mark a dimension `N/A` when omission is appropriate; do not reward generic boilerplate.
- When comparing before/after versions, use the same rubric, same validator, and same scenario assumptions for both versions.
- Do not infer live task performance from static review. Name the remaining uncertainty unless representative prompts were tested.

## Resources

- [references/basic-rubric.md](references/basic-rubric.md): required scoring dimensions, anchors, adjustment rules, and confidence definitions.
