# figma-spec-to-ui-1to1

一个同时支持 Codex skill 与 Kiro power 的 Figma 高保真还原仓库。

它的目标不是“拿到设计稿就直接生成一版差不多的页面”，而是强制 AI 按照“先整理规格、再给开发方案、最后再写代码”的顺序推进，把 Figma 链接、node-id、截图观察、人工规格补充，转成更稳定、更接近 1:1 的页面说明与前端实现结果。

## 快速安装

### Codex

在 Codex 中输入：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/IUExcellently/figma-spec-to-ui-1to1/refs/heads/main/.codex/INSTALL.md
```

更详细的 Codex 安装说明见 `docs/README.codex.md`。

### Kiro

在 Kiro IDE 中：

1. 打开 Powers 面板
2. 选择 `Add power from GitHub`
3. 输入仓库地址 `https://github.com/IUExcellently/figma-spec-to-ui-1to1`

更详细的 Kiro 安装说明见 `docs/README.kiro.md`。

## 项目介绍

`figma-spec-to-ui-1to1` 是一个偏流程约束型的仓库，适合以下场景：

- 你已经有 Figma 设计稿，但 AI 生成的页面总是“形似而神不似”
- 一个大节点下同时包含默认态、弹窗态、浮层态、hover 态、loading 态，AI 很容易误判结构
- 你希望 AI 先把页面结构、状态边界、定位参考系、组件库覆盖策略说清楚，再进入代码实现
- 你希望把 Figma 到代码的过程沉淀成一套可复用的团队规范，而不是一次性提示词

这个仓库默认强调高保真、结构优先、状态边界清晰、可继续开发，不鼓励为了“工程优雅”牺牲设计还原度。

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
- 双平台包装：同一仓库根目录同时支持 Codex 的 `SKILL.md` 与 Kiro 的 `POWER.md`

## 目录结构

```text
.
├── .codex
│   └── INSTALL.md
├── LICENSE
├── POWER.md
├── README.md
├── SKILL.md
├── docs
│   ├── README.codex.md
│   └── README.kiro.md
├── examples
│   ├── example-input.md
│   └── example-output.md
├── references
│   ├── create-doc-prompt-template.md
│   ├── development-checklist.md
│   ├── figma-input-template.md
│   └── prompt-template.md
└── steering
    ├── 01-input-and-scope.md
    ├── 02-structure-and-layout.md
    └── 03-implementation-and-check.md
```

各文件用途如下：

- `SKILL.md`：Codex 使用的 skill 主体规则
- `POWER.md`：Kiro 使用的 custom power 入口文件
- `.codex/INSTALL.md`：给 Codex 使用的安装说明
- `docs/README.codex.md`：Codex 平台说明
- `docs/README.kiro.md`：Kiro 平台说明
- `references/figma-input-template.md`：用于整理 Figma 输入材料
- `references/prompt-template.md`：给 Codex、Kiro 等工具复用的统一提示词模板
- `references/create-doc-prompt-template.md`：用于触发“先生成 create.md 级详细设计文档，再生成代码”的输入模板
- `references/development-checklist.md`：代码交付前的高保真自检清单
- `steering/`：Kiro power 按工作流拆分的指导文件
- `examples/example-input.md`：示例输入
- `examples/example-output.md`：示例输出

## 如何在 Codex 中使用

推荐把这个仓库作为一个独立 skill 目录使用。Codex 会读取根目录中的 `SKILL.md`。

### 方式一：通过安装说明接入

1. 按 `docs/README.codex.md` 或 `.codex/INSTALL.md` 中的步骤安装。
2. 在任务中明确要求使用 `figma-spec-to-ui-1to1`。
3. 将 Figma 链接、node-id、页面结构、补充规则等信息一起提供给 Codex。
4. 优先使用 `references/figma-input-template.md` 整理输入，避免信息缺失。
5. 如果你希望 AI 先生成 `create.md` 级详细文档，再继续写代码，优先直接复制 `references/create-doc-prompt-template.md`。

可直接参考下面这类触发方式：

```text
请使用 figma-spec-to-ui-1to1。
我会提供 Figma 链接、node-id、页面结构和补充规格。
请先输出结构化 UI 设计说明书，再输出开发方案，最后再生成代码。
```

### 方式二：作为工作区规则使用

如果你当前不是通过本地 skills 机制加载，也可以把 `SKILL.md` 的核心规则直接放入会话上下文，或让 Codex 读取当前目录中的 `SKILL.md` 与 `references/` 文件，再开始执行任务。

## 如何在 Kiro 中使用

Kiro 使用根目录中的 `POWER.md` 识别这是一个 custom power，并根据其中的关键词与 steering 映射动态加载上下文。

推荐做法如下：

1. 在 Kiro IDE 的 Powers 面板中，通过 GitHub URL 或本地路径安装当前仓库。
2. 安装后，Kiro 会读取 `POWER.md`。
3. 当任务命中关键词时，Kiro 会按需加载 `steering/` 中的工作流文件。
4. 若输入复杂，先按 `references/figma-input-template.md` 整理后再提交给 Kiro。
5. 若目标是先生成 `create.md` 级详细文档，再继续写代码，优先直接使用 `references/create-doc-prompt-template.md`。
6. 代码生成完成后，再用 `references/development-checklist.md` 做一次复核。

如果你在 Kiro 中只想先拿规格说明，不想直接生成代码，也可以明确要求“只执行到第二阶段”或“执行到第三阶段停止”。

## 推荐使用流程

1. 收集输入材料：Figma 链接、node-id、截图、业务规则、响应式要求、组件库约束。
2. 用 `references/figma-input-template.md` 把材料整理成结构化输入。
3. 若目标是高质量交付，优先直接使用 `references/create-doc-prompt-template.md` 作为输入提示词。
4. 在 Codex 中使用 `SKILL.md`，在 Kiro 中使用 `POWER.md` 与 `steering/`。
5. 先拿到 create.md 级详细设计文档，确认模块树、状态边界、浮层归属、定位参考系、接口依赖和动态规则是否正确。
6. 再让 AI 输出开发方案，确认组件拆分、样式覆盖策略、滚动容器和高风险点。
7. 最后再生成代码，并用 `references/development-checklist.md` 做交付前自检。

## 示例说明

仓库内提供了一组简化示例，方便理解这个仓库的实际输入输出形式：

- `examples/example-input.md`：展示如何组织一个真实可执行的输入，包括 Figma 链接、多 node-id、状态稿归类和模块规格补充
- `examples/example-output.md`：展示 AI 在使用本仓库规则后，推荐产出的结构和粒度
- `references/create-doc-prompt-template.md`：展示如何直接触发“先生成 create.md 级详细设计文档，再生成代码”的流程

这两个示例都是“缩略版”，用于展示格式与思路，不代表真实项目中的完整输出长度。

## 已知限制

- 该仓库不能替代完整的设计验收，最终高保真仍然需要人工复核
- 如果输入材料缺失较多，AI 仍然只能通过 `【推断值】` 补齐，无法凭空还原真实设计
- 是否能直接读取 Figma 节点信息，取决于你当前所用平台是否具备对应能力
- 默认技术栈示例偏向 `Vue 3 + TypeScript + Arco Design + script setup`，如果项目栈不同，需要在输入里显式说明
- 该仓库更擅长约束流程和输出结构，不负责自动安装字体、自动同步切图或自动修复第三方组件库缺陷
- `SKILL.md` 与 `POWER.md` 的表达方式面向不同平台，规则目标一致，但不会逐行完全同步
- 大节点、多状态、多浮层场景下，首轮最好先确认“哪些内容进入本次开发范围”，否则输出会变得很长

## License 说明

本项目采用 Apache License 2.0 开源。

- 你可以在保留 License 条款的前提下使用、修改和再分发本仓库内容
- 若你分发修改后的版本，应保留相关版权与许可说明
- 详细条款见根目录下的 `LICENSE`

如果你希望把这个仓库继续扩展成团队内部规范库，建议保留 `SKILL.md`、`POWER.md`、`references/`、`steering/` 和 `examples/` 的整体结构，方便后续维护与复用。
