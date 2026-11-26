# Codex 指令清理

[English README](README.md)

`agent-env-cleanup` 是一个 Codex 技能，用于在指令集变得嘈杂或模型、工作流发生变化时，审计 `AGENTS.md`、Skills 及其引用的指令文件。它用文件和行号证据记录重复或冲突规则；获得授权后，只做保留项目要求的最小范围修改。

它适合审查项目指令栈、合并重复说明、收紧过宽触发条件，或把细节移到相关引用中。请求可以明确指定 `~/.codex/skills` 等更大目录；未指定时，默认只读审计当前项目。

不要用它清理缓存、会话、凭据、模型运行时配置、插件上游文件或产品代码。先读 [SKILL.md](SKILL.md)，其中定义了触发条件、范围、证据、决定和完成检查。

采用 [MIT 许可证](LICENSE)。

## 示例请求

```text
$agent-env-cleanup 审查当前项目的 AGENTS.md 和 Skills，返回有文件证据的具体发现和修改建议。
```

```text
$agent-env-cleanup 清理指定的 AGENTS.md 和 .agents/skills；保留安全、数据完整性、证据与项目验收要求，然后验证。
```

## 从 GitHub 克隆

先克隆仓库，再使用 Codex 原生技能加载器安装目录。本技能是无安装脚本的 Markdown 目录。

```bash
git clone https://github.com/WhoJay0609/codex-instruction-cleanup.git
cd codex-instruction-cleanup
```

然后执行下面的本地安装命令。命令会创建符号链接；目标已存在时不会覆盖。

## 本地安装

请在仓库根目录执行（兼容 Linux 和 macOS）。命令会先检查目标路径是否已存在文件或符号链接（包括悬空符号链接），避免覆盖已有目标。

```bash
codex_skill_dir="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$codex_skill_dir"
skill_link="$codex_skill_dir/agent-env-cleanup"
if [ -e "$skill_link" ] || [ -L "$skill_link" ]; then
  printf 'Refusing to replace existing %s\n' "$skill_link" >&2
  exit 1
fi
ln -s -- "$PWD" "$skill_link"
```

`CODEX_HOME` 控制目标目录；未设置时使用 `~/.codex`。安装后任务没有列出该技能时，可开启新任务，或让代理直接读取本仓库的 `SKILL.md`。

## 审计输出

报告应让证据和决定可见：

```text
范围：检查过的路径和编辑授权
发现：
- path/to/file:42 — keep|delete|merge|move|rewrite — 原因和证据
修改：具体文件和编辑，或无
检查：命令和结果
风险：未解决的边界或未知项，或无
```

发现要引用实际路径和行号，说明原因，并写明修改及验证结果。相似文本只是待审候选，不能单独证明存在冲突或 token 节省。

## 示例规则改写

仅作说明，并非实测结果：“每次编辑前阅读完整地图” → “任务需要时查阅相关项目文档，并保留适用的指令规则”；“一直测试到确定” → “运行受编辑影响的检查，并保留项目要求的门禁”。

## 边界

- 审计默认只读。明确请求可以授权在指定目录或工作流内进行必要编辑。对于更大范围、共享目标或破坏性步骤，先核对现有授权，只询问未覆盖部分。
- 保留安全、数据完整性、项目验收、证据边界和必要的错误处理要求。
- 编辑前检查当前生效与仅在磁盘上的指令、别名和共享链接、生成文件及插件托管文件。
- 本技能依据本地指令文件和链接工作，不清理主机缓存，也不做运行时内省；不承诺 token、时延、质量或搜索可发现性收益。

## 兼容性与来源

本文档面向 Codex 原生技能加载器，触发名保持为 `agent-env-cleanup`（`$agent-env-cleanup`）。Claude Code 用法尚未测试，不宣称原生支持。

- [SKILL.md](SKILL.md)：本地触发、流程和安全规则。
- [references/sources.md](references/sources.md)：来源摘要及其边界。
- [GPT-6 Astra testing and verification](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra#gpt-6-astra-testing-and-verification)：本地引用的官方指导。
- [Rethinking skills and prompts for GPT-6 Astra](https://x.com/pvncher/status/2095991462416490862)：本地引用的用户提供文章；此处未重新核验其远程页面。
