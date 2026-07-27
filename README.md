# Skill Basic Scorer

An evidence-based Codex skill for manually reviewing, comparing, and scoring Agent Skills.

The included 100-point rubric evaluates:

- discoverability and metadata;
- scope and trigger fit;
- outcomes and success criteria;
- workflow judgment and degrees of freedom;
- prompt economy and progressive disclosure;
- evidence and missing-context behavior;
- output contracts and stop rules;
- validation and benchmark guidance;
- safety and operational guardrails.

> [!IMPORTANT]
> This is a custom review heuristic aligned with OpenAI guidance and the Agent Skills specification. It is not an OpenAI certification or official scoring system.

## Install as a plugin

Add this GitHub repository as a Codex plugin marketplace:

```bash
codex plugin marketplace add Theonefkguy/skill-basic-scorer --ref main
```

Restart the ChatGPT desktop app, open **Plugins**, select **Theonefkguy Skills**, and install **Skill Basic Scorer**.

## Install the standalone skill

In Codex, invoke `$skill-installer` with:

```text
Install https://github.com/Theonefkguy/skill-basic-scorer/tree/main/plugins/skill-basic-scorer/skills/skill-basic-scorer
```

## Usage

Invoke the skill explicitly:

```text
$skill-basic-scorer Review this SKILL.md
```

Other examples:

```text
$skill-basic-scorer Compare these two Skill versions.
$skill-basic-scorer Audit every direct child Skill in this directory.
```

The result separates:

- Agent Skills specification requirements;
- current OpenAI product guidance;
- custom rubric judgment;
- structural validator results;
- live benchmark evidence, when available.

## Repository layout

```text
.
├── .agents/plugins/marketplace.json
└── plugins/skill-basic-scorer
    ├── .codex-plugin/plugin.json
    └── skills/skill-basic-scorer
        ├── SKILL.md
        ├── agents/openai.yaml
        └── references/basic-rubric.md
```

## License

MIT
