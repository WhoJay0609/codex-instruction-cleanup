# Codex Instruction Cleanup

[中文说明](README.zh-CN.md)

`agent-env-cleanup` is a Codex skill for auditing `AGENTS.md`, Skills, and their referenced instruction files when an instruction set has grown noisy or a model or workflow changes. It records duplicate or conflicting rules with file and line evidence and, when authorized, applies the smallest scoped edit that preserves project requirements.

Use it to review a project's instruction stack, merge repeated guidance, tighten broad triggers, or move detail into a relevant reference. A request may name a broader directory such as `~/.codex/skills`; without that scope, the default is a read-only audit of the current project.

Do not use it to clean caches, sessions, credentials, model runtime configuration, plugin upstream files, or product code. Start with [SKILL.md](SKILL.md), which defines the trigger, scope, evidence, decisions, and completion checks.

Licensed under the [MIT License](LICENSE).

## Example requests

```text
$agent-env-cleanup Review this project's AGENTS.md and Skills. Return concrete findings and suggested edits.
```

```text
$agent-env-cleanup Clean up the named AGENTS.md and .agents/skills files; preserve safety, data-integrity, evidence, and acceptance requirements, then validate.
```

## Clone from GitHub

Clone the repository and then install the directory with Codex's native skill loader. This skill is a script-free Markdown directory; no installer script is needed.

```bash
git clone https://github.com/WhoJay0609/codex-instruction-cleanup.git
cd codex-instruction-cleanup
```

Continue with the local installation command below. It creates a symlink and does not overwrite an existing destination.

## Install locally

Run this from the repository root (works on Linux and macOS). It checks for an existing file or symlink (including a dangling symlink) before creating the link so it never overwrites an existing destination.

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

The `CODEX_HOME` value controls the destination; when it is unset, the command uses `~/.codex`. If a task does not list the skill after installation, start a new task or point the agent to this repository's `SKILL.md`.

## Review output

A useful report keeps evidence and decisions visible:

```text
Scope: paths inspected and edit authority
Findings:
- path/to/file:42 — keep|delete|merge|move|rewrite — reason and evidence
Changes: exact files and edits, or none
Checks: commands and results
Risks: unresolved boundary or unknown, or none
```

Findings should cite the actual path and line, explain the reason, and state the edit and validation result. Similar text is a candidate for review, not proof of a conflict or a token saving.

## Illustrative rule rewrite

Illustrative only; this is not a measured result: “read the whole map before every edit” → “consult relevant project docs when the task requires them, and preserve applicable instruction rules”; “test till sure” → “run checks affected by the edit and retain required project gates.”

## Boundaries

- An audit is read-only by default. An explicit request may authorize necessary edits within a named directory or workflow. Before a broader, shared-target, or destructive step, check the authorization and ask only about the uncovered scope.
- Preserve safety, data-integrity, project-acceptance, evidence, and necessary error-handling requirements.
- Check active versus disk-only instructions, aliases and shared links, generated files, and plugin-managed files before editing.
- This skill works from local instruction files and links. It does not perform host cache cleanup or live runtime introspection, and it makes no token, latency, quality, or search-discoverability guarantee.

## Compatibility and sources

The documented target is the native Codex skill loader, and the trigger remains `agent-env-cleanup` (`$agent-env-cleanup`). Claude Code usage has not been tested and is not advertised as supported.

- [SKILL.md](SKILL.md): local trigger, workflow, and safety rules.
- [references/sources.md](references/sources.md): source summaries and their boundaries.
- [GPT-6 Astra testing and verification](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra#gpt-6-astra-testing-and-verification): official guidance linked by the local references.
- [Rethinking skills and prompts for GPT-6 Astra](https://x.com/pvncher/status/2095991462416490862): user-provided article linked by the local references; its remote page was not re-verified here.
