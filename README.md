# ai-workflow-supervisor

`ai-workflow-supervisor` 是一个用于多 AI 协作的总控审查工作流 skill。

它的核心思路是：高能力 AI 负责读上下文、定路线、拆阶段、审查结果和守边界；执行 AI 负责明确、可验证、可停止的具体实现任务。

## 适用场景

- 全新项目起步，需要先列整体计划、技术路线、目录框架和阶段边界。
- 用户把任务交给另一个 AI 执行，需要本 skill 负责审查交付结果。
- 需要把模糊需求拆成可复制给执行 AI 的任务书。
- 需要减少“执行 AI 自认为完成，但审查时发现很多问题”的信息差。
- 需要明确停止点，防止执行 AI 自行进入下一阶段。

## 不适用场景

- 用户要求当前 AI 直接完成全部代码实现。
- 只是回答一个简单事实，不涉及多 Agent 协作。
- 没有执行 AI、阶段门禁或交付审查需求。

## 文件结构

```text
ai-workflow-supervisor/
├── SKILL.md
└── agents/
    └── openai.yaml
```

## 使用方式

把本仓库内容放入支持 skills 的本地目录后，在对话中显式调用：

```text
[$ai-workflow-supervisor](path/to/ai-workflow-supervisor/SKILL.md)
```

或使用类似触发语：

```text
启用 ai-workflow-supervisor。
用总控审查官模式。
用贵模审查-便宜执行这个工作流。
```

## 工作流概要

1. 判断当前模式：全新项目起步、交付审查、修复门禁或下一阶段放行。
2. 全新项目先由总控 AI 规划项目框架，不急着交给执行 AI。
3. 任务足够具体后，生成包含允许范围、禁止范围、成功标准、审查重点、验收命令和停止点的执行指令。
4. 执行 AI 完成后停止，交回总控 AI 审查。
5. 总控 AI 根据证据决定通过、有条件通过或不通过。

## 分发包

`.skill` 分发包只应包含 skill 运行所需文件，例如：

- `SKILL.md`
- `agents/openai.yaml`

README 是 GitHub 仓库说明文件，不需要放进 `.skill` 包。