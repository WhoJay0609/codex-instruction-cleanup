# Agent Environment Cleanup

用于模型升级后审计和精简 `AGENTS.md`、Skills 与任务提示词的 Codex 技能。综合 OpenAI 官方指南和用户提供的《Rethinking skills and prompts for GPT-6 Astra》，保留真实约束，减少重复指令、误触发和不必要的流程。

## 使用

```text
$agent-env-cleanup 审计当前项目的 AGENTS.md 和项目 Skills，只给出具体修改建议。
```

```text
$agent-env-cleanup 清理当前项目的 AGENTS.md 和 .agents/skills：合并重复规则、收紧描述、按需拆分引用；保留项目验收与安全要求，完成修改并验证。
```

目标不清楚时默认只检查当前项目。全局目录需要在请求中指定，例如 `~/.codex/skills`。本技能整理指令文件，不清空缓存、会话、凭据或运行配置。

## 安装

将仓库目录放入 Codex 技能目录，或创建符号链接。下面命令在目标已存在时不会覆盖它：

```bash
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
ln -sT "$(pwd)" "${CODEX_HOME:-$HOME/.codex}/skills/agent-env-cleanup"
```

请在本仓库根目录执行。若当前任务的技能列表没有刷新，在新任务中使用；也可直接让代理读取本仓库的 `SKILL.md`。

## 文件

- [SKILL.md](SKILL.md)：触发条件、审计与清理流程、完成标准。
- [references/sources.md](references/sources.md)：两份来源的摘要、适用边界与来源链接。

无需额外依赖。创建时使用 Codex 自带的 `skill-creator/scripts/quick_validate.py` 校验格式；格式校验不等于实际任务效果测试。
