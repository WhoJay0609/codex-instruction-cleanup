# 来源与边界

## 用户提供文章

- 标题：*Rethinking skills and prompts for GPT-6 Astra*。
- X 链接：[原帖](https://x.com/pvncher/status/2095991462416490862)。内容来自用户粘贴文本，远程页面未复核；图片和示例不在文本中。
- 文章主张：缩短描述，按需渐进披露；让 AGENTS 指令保持上下文相关；减少僵化流程；适配混合模型；明确完成条件并在模型升级后复审。

## 官方指导（2026-09-12 核对）

来源：[GPT-6 Astra testing and verification](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-6-astra#gpt-6-astra-testing-and-verification)

- 审计会改变行为的指令。
- 用户指令优先于技能；系统/开发者指令仍处于更高层级。
- 已授权的工作应继续完成，只为未覆盖的实质权限提问。
- 材料未知会影响结果时先澄清；委派按任务与 harness 需要配置，不是硬性步骤。
- 保持简洁并做与风险相称的检查；只有出现相关改动、失败或疑虑时才扩大验证。

## 本技能新增的本地机制

优先级、加载态与磁盘态区分，别名/共享链接/生成及插件托管文件检查，恢复路径、逐文件证据和禁止自动删除，都是本地安全约束，不冒充上述来源的原文或结论。
