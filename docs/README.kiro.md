# figma-spec-to-ui-1to1 for Kiro

## 安装方式

### 方式一：从 GitHub 安装

1. 打开 Kiro IDE
2. 进入 Powers 面板
3. 选择 `Add power from GitHub`
4. 输入仓库地址：

```text
https://github.com/IUExcellently/figma-spec-to-ui-1to1
```

5. 点击安装

### 方式二：从本地路径安装

1. 将仓库克隆到本地
2. 在 Kiro Powers 面板选择 `Add power from Local Path`
3. 选择当前仓库根目录

Kiro 会读取根目录中的 `POWER.md`，并按其中的关键词与 steering 映射激活本 power。

## 目录说明

- `POWER.md`：Kiro custom power 的入口文件
- `steering/`：按工作流拆分的指导文件
- `references/`：平台无关的模板与检查清单
- 重要提醒：当 AI 真正启用这个 power 后，通常会显著增加 token 消耗，尤其是在多 node-id、多画板、多断点、多状态稿场景下
- 若你希望 AI 先生成 `create.md` 级详细文档，再继续写代码，优先直接使用 `references/create-doc-prompt-template.md`

## 激活场景

以下场景较容易触发本 power：

- 提到 Figma、node-id、设计稿还原、高保真、像素级、设计交付
- 需要把设计稿转成页面规格说明、开发方案和前端代码
- 需要处理多画板、多状态稿、弹窗态、浮层态或多 node-id 输入

## 推荐用法

在 Kiro 中描述任务时，尽量明确以下信息：

- Figma 链接
- node-id 或节点路径
- 页面结构描述
- 补充的样式 / 交互 / 响应式规则
- 是否需要只输出说明书，还是要继续输出代码

如果输入复杂，建议先参考 `references/figma-input-template.md` 整理后再提交给 Kiro。
