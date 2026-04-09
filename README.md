# figma-spec-to-ui-1to1

一个面向 Codex、Kiro 等 AI 编程助手的 Figma 高保真还原 skill。

它的目标不是“拿到设计稿就直接生成一版差不多的页面”，而是强制 AI 按照“先整理规格、再给开发方案、最后再写代码”的顺序推进，把 Figma 链接、node-id、截图观察、人工规格补充，转成更稳定、更接近 1:1 的页面说明与前端实现结果。

## 项目介绍

`figma-spec-to-ui-1to1` 是一个偏流程约束型的 skill，适合以下场景：

- 你已经有 Figma 设计稿，但 AI 生成的页面总是“形似而神不似”
- 一个大节点下同时包含默认态、弹窗态、浮层态、hover 态、loading 态，AI 很容易误判结构
- 你希望 AI 先把页面结构、状态边界、定位参考系、组件库覆盖策略说清楚，再进入代码实现
- 你希望把 Figma 到代码的过程沉淀成一套可复用的团队规范，而不是一次性提示词

这个 skill 默认强调高保真、结构优先、状态边界清晰、可继续开发，不鼓励为了“工程优雅”牺牲设计还原度。

## 解决什么问题

- 解决 AI 直接看图开写，导致页面层级、模块划分和布局意图被改写的问题
- 解决多个 node-id、多画板、多状态稿混在一起时，被误拼成一个静态页面的问题
- 解决只还原颜色和字号，却忽略间距、定位参考系、滚动区和交互边界的问题
- 解决 Arco Design 等组件库默认样式残留，导致页面落地与设计稿不一致的问题
- 解决 AI 把未知字体直接写进代码、把推断值伪装成设计原值的问题

## 核心能力

- 输入分层：区分全局规则、主布局规则、页面级规则、组件级规则
- 多 node-id 结构树解析：先还原父子关系、容器关系，再处理样式细节
- 多画板 / 多状态 / 浮层识别：先做归类，再做页面模块树和代码
- 布局意图推断：避免把双列、三列容器机械实现为等分布局
- 视觉归属 / DOM 挂载归属 / 定位参考归属拆分：降低 `absolute`、`fixed`、`sticky` 误判
- 组件库默认样式覆盖约束：适合 Arco Design 场景下的高保真实现
- `font-family` 忽略策略：默认不把 Figma 字体名直接写入前端代码
- 结构化输出：固定产出“说明书 -> 开发方案 -> 代码 -> 自检结论”的交付顺序

## 目录结构

```text
.
├── LICENSE
├── README.md
├── SKILL.md
├── examples
│   ├── example-input.md
│   └── example-output.md
└── references
    ├── development-checklist.md
    ├── figma-input-template.md
    └── prompt-template.md
```

各文件用途如下：

- `SKILL.md`：skill 主体规则，定义触发条件、固定工作流和输出要求
- `references/figma-input-template.md`：用于整理 Figma 输入材料
- `references/prompt-template.md`：给 Codex、Kiro 等工具复用的统一提示词模板
- `references/development-checklist.md`：代码交付前的高保真自检清单
- `examples/example-input.md`：示例输入
- `examples/example-output.md`：示例输出

## 如何在 Codex 中使用

推荐把这个仓库作为一个独立 skill 目录使用。

### 方式一：作为本地 skill 使用

1. 将整个目录放入你的 Codex skills 目录中，保留当前目录结构。
2. 在任务中明确要求使用 `figma-spec-to-ui-1to1`。
3. 将 Figma 链接、node-id、页面结构、补充规则等信息一起提供给 Codex。
4. 优先使用 `references/figma-input-template.md` 整理输入，避免信息缺失。

可直接参考下面这类调用方式：

```text
请使用 figma-spec-to-ui-1to1。
我会提供 Figma 链接、node-id、页面结构和补充规格。
请先输出结构化 UI 设计说明书，再输出开发方案，最后再生成代码。
```

### 方式二：作为工作区规则使用

如果你当前不是通过本地 skills 机制加载，也可以把 `SKILL.md` 的核心规则直接放入会话上下文，或让 Codex读取当前目录中的 `SKILL.md` 与 `references/` 文件，再开始执行任务。

## 如何在 Kiro 中使用

Kiro 不一定直接复用本地 skill 机制，因此更推荐把本仓库当作“提示词 + 参考模板 + 自检规范”来使用。

推荐做法如下：

1. 将 `SKILL.md` 作为项目级规则、会话前置说明，或粘贴到任务上下文中。
2. 将 `references/prompt-template.md` 作为主提示词骨架。
3. 先按 `references/figma-input-template.md` 填好你的页面输入。
4. 把整理后的输入和提示词一起交给 Kiro，要求它按固定四阶段输出。
5. 代码生成完成后，再用 `references/development-checklist.md` 做一次复核。

如果你在 Kiro 中只想先拿规格说明，不想直接生成代码，也可以明确要求“只执行到第二阶段”或“执行到第三阶段停止”。

## 推荐使用流程

1. 收集输入材料：Figma 链接、node-id、截图、业务规则、响应式要求、组件库约束。
2. 用 `references/figma-input-template.md` 把材料整理成结构化输入。
3. 将 `SKILL.md` 与 `references/prompt-template.md` 一起交给 Codex 或 Kiro。
4. 先拿到结构化 UI 设计说明书，确认模块树、状态边界、浮层归属、定位参考系是否正确。
5. 再让 AI 输出开发方案，确认组件拆分、样式覆盖策略、滚动容器和高风险点。
6. 最后再生成代码，并用 `references/development-checklist.md` 做交付前自检。

## 示例说明

仓库内提供了一组简化示例，方便理解这个 skill 的实际输入输出形式：

- `examples/example-input.md`：展示如何组织一个真实可执行的输入，包括 Figma 链接、多 node-id、状态稿归类和模块规格补充
- `examples/example-output.md`：展示 AI 在使用本 skill 后，推荐产出的结构和粒度

这两个示例都是“缩略版”，用于展示格式与思路，不代表真实项目中的完整输出长度。

## 已知限制

- 该 skill 不能替代完整的设计验收，最终高保真仍然需要人工复核
- 如果输入材料缺失较多，AI 仍然只能通过 `【推断值】` 补齐，无法凭空还原真实设计
- 是否能直接读取 Figma 节点信息，取决于你当前所用平台是否具备对应能力
- 默认技术栈示例偏向 `Vue 3 + TypeScript + Arco Design + script setup`，如果项目栈不同，需要在输入里显式说明
- 该 skill 更擅长约束流程和输出结构，不负责自动安装字体、自动同步切图或自动修复第三方组件库缺陷
- 大节点、多状态、多浮层场景下，首轮最好先确认“哪些内容进入本次开发范围”，否则输出会变得很长

## License 说明

本项目采用 Apache License 2.0 开源。

- 你可以在保留 License 条款的前提下使用、修改和再分发本仓库内容
- 若你分发修改后的版本，应保留相关版权与许可说明
- 详细条款见根目录下的 `LICENSE`

如果你希望把这个 skill 继续扩展成团队内部规范库，建议保留 `SKILL.md`、`references/` 和 `examples/` 的整体结构，方便后续维护与复用。
