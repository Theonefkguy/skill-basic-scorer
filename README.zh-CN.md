# Skill Basic Scorer

[English](README.md) | [简体中文](README.zh-CN.md)

一个基于证据的 Codex Skill，用于人工评审、比较和评分 Agent Skills。

内置的自定义 100 分评价体系涵盖：

- 可发现性与元数据；
- 范围与触发匹配；
- 目标与成功标准；
- 工作流判断与自由度；
- 提示词经济性与渐进式披露；
- 证据与上下文缺失处理；
- 输出契约与停止规则；
- 验证与基准测试指导；
- 安全与操作护栏。

> [!IMPORTANT]
> 这是一套与 OpenAI 指南和 Agent Skills 规范对齐的自定义评审方法，不是 OpenAI 官方认证或官方评分体系。

## 作为 Plugin 安装

将这个 GitHub 仓库添加为 Codex Plugin marketplace：

```bash
codex plugin marketplace add Theonefkguy/skill-basic-scorer --ref main
```

重启 ChatGPT 桌面应用，打开 **Plugins**，选择 **Theonefkguy Skills**，然后安装 **Skill Basic Scorer**。

## 安装独立 Skill

在 Codex 中调用 `$skill-installer`，并输入：

```text
Install https://github.com/Theonefkguy/skill-basic-scorer/tree/main/plugins/skill-basic-scorer/skills/skill-basic-scorer
```

## 使用方法

显式调用这个 Skill：

```text
$skill-basic-scorer 评审这个 SKILL.md
```

其他示例：

```text
$skill-basic-scorer 比较这两个 Skill 版本，并解释评分变化。
$skill-basic-scorer 审计这个目录下所有直接子级 Skill。
```

评审结果会明确区分：

- Agent Skills 规范要求；
- 当前 OpenAI 产品指南；
- 自定义 rubric 判断；
- 结构验证器结果；
- 实际基准测试证据（如果存在）。

## 仓库结构

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

## 许可证

MIT
