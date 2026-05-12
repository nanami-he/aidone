[English](./README.md) | **简体中文**

# AIDONE

**不要相信"完成了"，要证据。**

AIDONE 是一份面向产品方的 AI 代码验收协议。它给创业者、产品人、设计师、创作者、独立开发者用，让你能在不看每一行代码的情况下，判断 AI 写出来的代码是不是真的可以交给用户。

核心文件是 [`AIDONE.md`](./AIDONE.md)：一份放在项目根目录的清单，让 AI coding agent 在说"完成了"之前，按它来交付。协议本身保持英文一份——这一档文件（类似 `llms.txt`、`AGENTS.md`）业界惯例都是单语英文，AI agent 读英文也更稳。

## 前后对比

没用 AIDONE 之前，AI agent 收尾大概长这样：

```text
完成了。Settings 页面已经实现，可以使用。
```

用了 AIDONE 之后，同一个交付应该长这样：

```text
产品结果：Settings 页面现在支持导出数据和清除本地数据。
改了哪些文件：SettingsView.tsx、db/export.ts、copy.ts。
AIDONE：P0 安全项通过（无密钥泄露，输入有校验）。P1 缺口：清除数据这个破坏性操作还缺二次确认。本项目没有测试运行环境，没补测试。
验证：pnpm build 通过。pnpm lint 在 2 个无关旧文件上失败。
手动验收：Settings -> Export Data，确认 JSON 下载。
残余风险：清除数据的破坏性流程在给用户用之前应该再收紧一次。
```

重点不是让 AI 多说几句话，而是要证据。

## 为什么做这个

AI 写出来的代码能跑。

不代表它真的可以交给用户。

一个功能看起来"能用"，但底下可能漏了一堆基本盘：

- 用户能看到的文字全写死在代码里，以后没法改语言、改文案
- 加载中、空状态、错误、成功状态，全都没做
- API key、密码、URL 直接写在代码里
- 权限只在前端做隐藏，后端没拦
- 用户输错东西就崩
- 错误页面直接把堆栈甩给用户看
- 没有测试
- agent 张嘴就说"可以上线"，但 build / lint / 测试一个都没跑

AIDONE 把这些藏起来的工程默认值，变成明确的验收项。

## 验收，不是 code review

AIDONE 不是又一份给工程师炫技的最佳实践清单。

Code review 问的是："这段代码写得好不好？"

AIDONE 问的是："产品负责人在不读每一行代码的情况下，能不能验收这份 AI 写出来的活儿？"

这是两个不同的活。AIDONE 把工程关切翻译成产品方能问出口的问题：

- 以后要改语言、改文案，文字放在哪儿？
- API 挂了，用户看到的是什么？
- 普通用户去戳一下管理员路径，会被什么挡住？
- agent 说"测试通过了"，是哪条命令证明的？
- 有破坏性操作的话，靠什么防止误触？

## 谁该用

如果你符合下面这些，就该用 AIDONE：

- 用 AI agent vibe coding
- 不会一行一行 review 代码
- 是创业者、产品人、设计师、创作者，或者不专门写代码的独立开发者
- 但你还是希望产出有专业软件的基本盘
- 你想要证据，而不是 AI 嘴上的"完成了"

它不能代替资深工程师的人工 review。它是一条务实的验收线，让 AI agent 主动报风险、给证据、说清楚哪些没跑。

## 上手

最简单的方式：把这个仓库地址扔给你的 AI 工具，让它自己装。

```text
帮我把 https://github.com/nanami-he/aidone 装到当前项目里。
```

Claude Code、Cursor、Codex、Aider 这类工具能直接读 GitHub URL，会把 AIDONE 协议文件抓下来、放到合适的位置、并把那段使用提示写进你的 `AGENTS.md` / `CLAUDE.md`。

想手动来也行：

把 [`AIDONE.md`](./AIDONE.md) 复制到你的项目根目录。

然后把下面这段加到你的 `AGENTS.md`、`CLAUDE.md`、Cursor 规则、或者项目说明里：

```md
在说任何一项编码任务"完成了"之前，先按 AIDONE.md 走一遍。

默认按 P0 和 P1 检查。
当任务涉及生产环境、数据迁移、外部服务、后台任务、支付、鉴权、或用户数据时，加上相关的 P2。
如果某项检查跑不了，明确说明原因，并给出最接近的替代验证。
```

让 AI agent 帮你审项目时：

```text
用 AIDONE 审一下这个项目。不要只看功能能不能跑。要按 P0、P1，以及相关的 P2 报缺口，附上文件位置和验证证据。
```

让 AI agent 帮你写代码时：

```text
按 AIDONE 实现这个需求。完成时报告：改了哪些文件、跑了哪些检查、P0/P1 的验收状态、用户怎么手动验收、还剩什么风险。
```

只想要简短回复时，加一句"用压缩版交付"：

```text
按 AIDONE 做，但交付用压缩版：证据、缺口、手动验收。
```

期望的形状：

```text
Evidence: pnpm build 通过；pnpm lint 在 2 个旧文件上失败。
Gaps: 没有测试；破坏性操作还缺二次确认。
Manual check: Settings -> Export Data 可以下载 JSON。
```

## 文件清单

```text
AIDONE.md                 协议本体（英文）
llms.txt                  LLM 可读的项目索引
examples/AGENTS.md        AGENTS.md 样例
examples/CLAUDE.md        Claude Code 项目说明样例
examples/review-prompt.md 项目审查 prompt
examples/delivery-template.md 交付模板（完整版 + 压缩版）
checklists/               专项深入清单
references/sources.md     参考资料与相关标准
```

## 优先级分档

| 档位 | 含义 | 什么时候用 |
|---|---|---|
| P0 | 永远要做 | 任何非琐碎的代码改动，包括原型 |
| P1 | 默认产品线 | 正常的产品工作：UI、API、鉴权、数据、设置、工作流 |
| P2 | 风险相关 | 生产环境、迁移、后台任务、外部服务、规模化场景 |

## 相关标准

AIDONE 属于 AI 时代正在形成的一组根目录文件：

| 文件 | 给谁看 | 作用 |
|---|---|---|
| `README.md` | 人 | 这是个什么项目 |
| `AGENTS.md` | coding agent | 在这个仓库里怎么工作 |
| `DESIGN.md` | design agent | 这个产品该长什么样、给人什么感觉 |
| `AIDONE.md` | coding agent 和审查者 | "完成了"得证明什么 |

## 许可

MIT
