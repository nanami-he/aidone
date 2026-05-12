# Launch Notes

Use these snippets when publishing AIDONE.

## One-Liner

AIDONE is a product-facing acceptance checklist for AI-generated code.

## Short Pitch

AI can write code that runs. That does not mean the work is acceptable.

AIDONE helps founders, product people, designers, creators, and solo builders ask AI coding agents for evidence before accepting "done".

## Tagline Options

- Don't trust "done". Ask for evidence.
- AI says done. AIDONE asks for proof.
- A checklist for accepting AI-generated code before you ship it.
- For people who vibe code but still need professional software defaults.

## GitHub Description

Product-facing acceptance protocol for AI-generated code. Don't trust "done". Ask for evidence.

## Social Post

I made AIDONE.md.

It's a checklist for accepting AI-generated code before you ship it.

AI agents are good at saying "done". But working code can still miss basic product engineering defaults: secrets, auth, i18n, loading/error states, validation, tests, docs, and verification evidence.

Before:

> Done. I implemented it and everything should work now.

After AIDONE:

> Build passed. Lint failed on 2 unrelated files. No tests exist in this project.
> P0 safety: pass.
> P1 gap: error state missing on the export view.
> Manual check: Settings → Export Data → confirm JSON downloads.
> Remaining risk: destructive clear-data action still needs a second confirmation.

Same task, same agent. AIDONE turns those hidden engineering expectations into explicit checks.

For founders, product people, designers, creators, and solo builders who vibe code but don't review every line of code.

Don't trust "done". Ask for evidence.

## Chinese Post

我整理了一份 `AIDONE.md`。

它不是给工程师炫技的 code review 清单，而是给 vibe coding 的产品人、创作者、独立开发者用的 AI 代码验收协议。

AI 很会说"完成了"。
但代码能跑，不代表它真的适合交给用户。

它可能没有 i18n、没有 loading / empty / error、没有权限边界、没有输入校验、没有测试、没有构建证据，甚至把错误堆栈直接展示给用户。

以前 AI 交付：

> 完成了，应该可以用了。

用了 AIDONE 之后：

> build 通过，lint 在两个无关文件上失败，项目里没有测试。
> P0 安全项：通过。
> P1 缺口：导出页缺一个错误状态。
> 手动验收：Settings → Export Data → 确认 JSON 下载。
> 残余风险：清除数据是破坏性操作，还需要二次确认。

同一个任务，同一个 agent。区别就在"要不要给证据"。

`AIDONE.md` 做的事很简单：

不要相信 done，要求证据。

让 AI 在交付前回答：

- 改了什么
- 改了哪些文件
- 跑了哪些验证
- P0/P1/P2 哪些通过、哪些没过
- 用户怎么手动验收
- 还有什么风险

这是给 AI 时代项目准备的一个根目录标准文件。

`README.md` 给人看。
`AGENTS.md` 给 coding agent 看。
`DESIGN.md` 给 design agent 看。
`llms.txt` 给外部 LLM 看。
`AIDONE.md` 定义 AI 说 done 之前必须证明什么。
