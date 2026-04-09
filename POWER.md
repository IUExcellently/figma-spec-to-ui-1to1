---
name: "figma-spec-to-ui-1to1"
displayName: "Figma Spec to UI 1:1"
description: "将 Figma 链接、node-id、截图观察与人工规格补充转成高保真、接近 1:1 的页面说明、开发方案与前端实现"
keywords:
  - "figma"
  - "Figma"
  - "node-id"
  - "design spec"
  - "high fidelity"
  - "pixel perfect"
  - "design handoff"
  - "ui spec"
  - "arco design"
  - "vue 3"
  - "设计稿"
  - "高保真"
  - "像素级"
  - "页面还原"
  - "设计还原"
  - "规格说明"
  - "node id"
---

# Onboarding

## Step 1: 判断任务是否适用

当任务满足以下任一条件时，启用本 power：

- 用户提供 Figma 链接、node-id、节点路径、截图观察结果
- 用户要求高保真、1:1、像素级、接近设计稿地还原页面
- 用户希望先输出规格说明、再输出开发方案、最后再生成代码
- 用户明确提到多画板、多状态稿、弹窗态、浮层态、多 node-id 混合输入

如果任务只是普通的低保真原型、创意发挥式页面设计，或不需要遵循设计稿约束，则不要使用本 power。

## Step 2: 明确本 power 的定位

本 power 是文档与流程约束型 power：

- 不提供 MCP 服务器
- 不要求额外 CLI 安装
- 不负责自动读取 Figma 节点
- 核心作用是约束 AI 的分析顺序、输出结构和高保真实现边界

## Step 3: 固定执行顺序

启用本 power 后，默认按以下顺序工作：

1. 先理解输入材料
2. 再输出结构化 UI 设计说明书
3. 再输出开发方案
4. 最后再生成代码
5. 交付前按高保真自检逻辑复核

除非用户明确要求只停留在前面的某个阶段，否则不要跳过前置阶段直接写代码。

# When to Load Steering Files

- 整理 Figma 输入、识别多 node-id / 多画板 / 多状态、处理信息优先级时，加载 `steering/01-input-and-scope.md`
- 还原页面结构树、布局角色、滚动区、定位参考系时，加载 `steering/02-structure-and-layout.md`
- 进入代码阶段、处理 Arco 默认样式覆盖、字体策略和最终自检时，加载 `steering/03-implementation-and-check.md`

# Default Rules

## 输出顺序

- 先结构化说明，再开发
- 不要直接根据截图自由发挥
- 不要因为代码复用或工程抽象牺牲还原度

## 推断值与冲突处理

- 缺失但又必须补齐的内容统一标记为 `【推断值】`
- 冲突信息要显式说明，不要默默选一个值直接实现
- 不要把推断值伪装成设计原值

## 高风险误判点

- 多个 node-id 输入时，不要默认拼成平级页面模块
- 一个大节点下出现多个 frame / section / 状态稿时，不要默认当成同一页面里的静态区块
- 视觉上在底部、顶部、右上角、右下角的元素，不要仅凭位置直接决定 `absolute`、`fixed` 或 `sticky`

## 字体策略

- 默认忽略 Figma 中的 `font-family` 原始值
- 不要在未确认授权的前提下把 Figma 字体名直接写进代码
- 优先还原字号、字重、行高、字距、对齐方式与文本容器关系

## 组件库策略

- 若使用 Arco Design，不要保留明显偏离设计稿的默认样式
- 需要显式说明输入类控件、按钮、弹窗、标签等默认样式覆盖项

# Recommended Deliverables

推荐交付顺序如下：

1. 输入材料理解结论
2. 信息来源与优先级说明
3. node-id 归类结果（若存在）
4. 页面结构树（若存在）
5. 画板归类结果（若存在）
6. 结构化 UI 设计说明书
7. 开发方案
8. 代码实现
9. 自检结论
10. 风险点、待确认项、推断值清单
