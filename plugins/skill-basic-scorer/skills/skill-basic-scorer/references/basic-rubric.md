# Basic Skill Scoring Rubric

Use this rubric for manual review. It is tuned for modern Codex skills: define the outcome, constraints, evidence, output, and stop rules; avoid inherited prompt machinery that narrows the model without improving correctness.

This is a custom quality rubric aligned with OpenAI guidance and the Agent Skills specification. It is not an OpenAI certification, official score, or substitute for representative task testing.

## 1. Discoverability and Metadata (12)

- 10-12: Valid frontmatter, correct `name`, concrete description with strong trigger phrases and task contexts.
- 6-9: Valid metadata but the description is generic, too short, or missing common trigger wording.
- 0-5: Invalid/missing frontmatter, unsupported top-level keys, TODO text, overlong descriptions, or unclear name/description.

Look for: `name`, `description`, folder-name match, "Use when..." style triggers, concrete file types/domains/tasks.

Spec anchors: `name` must be lowercase letters, digits, and hyphens; must be 1-64 characters; must not start/end with a hyphen; must not contain `--`; and must match the parent folder. `description` must be non-empty and no more than 1024 characters. Optional top-level fields such as `license`, `compatibility`, `metadata`, and `allowed-tools` are not defects when used according to the Agent Skills spec. `allowed-tools` is experimental, and support may vary by agent implementation.

## 2. Scope and Trigger Fit (10)

- 8-10: Clear primary purpose, boundaries, relevant handoff/fallback rules, and no major overlap confusion.
- 5-7: Mostly clear but broad, overlapping, or missing "when not to use" guidance.
- 0-4: Vague, grab-bag scope, or unclear relationship to adjacent skills.

Look for: domain specificity, exclusions, deprecated-shim clarity, adjacent skill handoffs when needed, and "use this skill when..." phrasing.

## 3. Outcome and Success Criteria (14)

- 12-14: Defines what good work looks like, what must be true before finalizing, and the constraints that matter.
- 7-11: Has a useful goal but success criteria, completion state, or important constraints are implicit.
- 0-6: Mostly process description, tips, or background with no clear target outcome.

Look for: outcome, goal, success criteria, quality bar, completion criteria, constraints, invariants, "final answer should contain", and examples that clarify good output rather than over-directing the path.

Penalize static keyword stuffing: many vague phrases like "follow best practices", "handle appropriately", "ensure quality", "robust", or "comprehensive" do not substitute for concrete task-specific success criteria.

## 4. Workflow Judgment and Degrees of Freedom (12)

- 10-12: Gives enough workflow guidance for reliable execution while leaving room for efficient, context-sensitive choices.
- 6-9: Useful steps exist, but decision rules, fallback paths, or degrees of freedom are weak.
- 0-5: Either too vague to execute or over-specified as a brittle legacy prompt stack.

Look for: decision rules, "prefer/default/when needed", tool-use guidance, fallback behavior, and true invariants. Penalize excessive `always`, `never`, `must`, `only`, or rigid step chains when they are not safety, data integrity, or output-format requirements.

## 5. Prompt Economy and Progressive Disclosure (12)

- 10-12: `SKILL.md` stays lean, routes details to references/scripts/assets only when needed, and avoids duplicating obvious model knowledge.
- 6-9: Acceptable size but either too much inline detail, weak reference-loading guidance, or some redundant prompting.
- 0-5: Main file is bloated, encyclopedic, or hides important details in unmentioned resources.

Calibration: file length is a diagnostic signal, not a quality threshold. Penalize length only when it creates duplicated context, hides routing decisions, or makes the workflow harder to follow. Short can be excellent when the outcome and boundaries are clear.

Resource-health anchors: every essential file in `references/`, `scripts/`, or `assets/` should be discoverable from `SKILL.md` with clear when-to-load or when-to-use guidance. Penalize broken links, hidden essential resources, or nesting that materially harms discovery; nesting alone is not a defect. For scripts, baseline static review should inspect affordances rather than execute arbitrary target code: look for documented dependencies, understandable inputs, predictable outputs, and useful failure behavior appropriate to the script.

## 6. Evidence, Retrieval, and Missing-Context Behavior (10)

- 8-10: Says what evidence is required, what counts as enough, when to retrieve/read more, and how to handle missing facts.
- 5-7: Mentions evidence, citations, assumptions, or caveats but lacks a clear retrieval budget or missing-context rule.
- 0-4: Encourages unsupported claims, exhaustive searching by default, or unclear behavior when evidence is absent.

Look for, when relevant: evidence standards, citation requirements, minimum sufficient evidence, source-backed claims, assumptions, blockers, asking for the smallest missing field, and avoiding unsupported negative conclusions.

Not every skill needs web retrieval. For local coding, document, or writing skills, score the analogous behavior: which files/context to inspect, when enough context is enough, and when to ask rather than invent.

Mark this dimension `N/A` when the workflow neither consumes factual evidence nor depends on missing external or local context, and omission does not reduce reliability.

## 7. Output Contract and Stop Rules (12)

- 10-12: Specifies final output shape, required fields or artifacts, caveats/blockers, and when to stop.
- 6-9: Mostly usable but missing output shape, path handling, or explicit stopping conditions.
- 0-5: Ambiguous, theory-only, or no clear completion condition.

Look for: output templates, report fields, artifact expectations, final answer contents, stop rules, blockers, and concise formatting guidance that matches the user experience.

## 8. Validation and Benchmark Guidance (9)

- 8-9: Includes validation commands, smoke tests, self-checks, rendering checks, or representative benchmark protocol appropriate to the domain.
- 5-7: Mentions checking results but lacks concrete commands or evidence standards.
- 0-4: No validation path.

For skills, `quick_validate.py` is structural only; live task benchmarks are stronger evidence. For baseline static scoring, also reward representative examples, comparison anchors, or explicit self-checks that make manual judgment more consistent. When validation cannot run, the skill should say how to report that limitation.

Mark this dimension `N/A` only when the output has no meaningful correctness or quality check. Do not require a command-line test when an appropriate review, example, or output check is sufficient.

## 9. Safety and Operational Guardrails (9)

- 8-9: Handles destructive actions, secrets, privacy, user changes, uncertainty, and permission boundaries where relevant.
- 5-7: Some guardrails but gaps in risky workflows.
- 0-4: Unsafe defaults, overclaims, secret exposure risk, or destructive actions without confirmation.

Safety should fit the domain. A writing skill may need spoiler/clean-room boundaries; an ops skill needs backups, confirmation, and redaction; an analysis skill needs uncertainty and evidence caveats.

Mark this dimension `N/A` when the workflow has no material destructive, privacy, permission, secret, compliance, or harmful-output risk. Do not add generic safety boilerplate solely to earn points.

## Adjustment Rules

- Assign each dimension directly from its rubric anchors. When evidence falls between bands, choose the more conservative score and explain the deciding tradeoff rather than retrofitting a preferred total.
- Mark genuinely irrelevant dimensions `N/A` and normalize the total as `earned points / maximum applicable points × 100`. Explain every `N/A`; do not use it to hide a real gap.
- Mark scores as static review unless live task tests were run.
- For before/after comparisons, evaluate both versions with the same rubric and note tradeoffs, not only score gains.
- Prefer low-risk suggestions first: metadata fixes, broken links, outcome criteria, output contract, validation command, evidence budget, reference-loading notes.
- When assessing standards compliance, distinguish Agent Skills specification requirements, current OpenAI product guidance, custom rubric heuristics, validator output, and live benchmark evidence.
- Do not reward length or rigid process by default. Reward clear outcomes, useful constraints, and enough judgment guidance to let the active Codex model choose an efficient route.

## Confidence Labels

Assign confidence from the completeness and reliability of the inspected evidence:

- High: the target and required resources were fully inspected, validator status is known when relevant, and the evidence clearly supports the score. The skill may still score high or low.
- Medium: the target is substantially inspectable, but untested scripts, missing representative examples, or minor inaccessible resources could materially shift some dimensions.
- Low: essential resources or target content are inaccessible, the intended environment is ambiguous, critical dependencies cannot be assessed, or the score is otherwise likely to change substantially with missing evidence.

Confidence is not a quality grade. A low-scoring skill can have high confidence if the static evidence is clear, and a high-scoring skill can have medium confidence if hidden resources or script concerns make the score less stable.
