# figma-spec-to-ui-1to1 for Codex

## 快速安装

在 Codex 中输入：

```text
Fetch and follow instructions from https://raw.githubusercontent.com/IUExcellently/figma-spec-to-ui-1to1/refs/heads/main/.codex/INSTALL.md
```

## 手动安装

1. 克隆仓库：

```bash
git clone https://github.com/IUExcellently/figma-spec-to-ui-1to1.git ~/.codex/figma-spec-to-ui-1to1
```

2. 建立 skill 链接：

```bash
mkdir -p ~/.agents/skills
ln -s ~/.codex/figma-spec-to-ui-1to1 ~/.agents/skills/figma-spec-to-ui-1to1
```

3. 重启 Codex。

## 说明

- Codex 使用根目录中的 `SKILL.md` 进行技能发现
- `references/` 提供输入模板、统一提示词模板和交付前自检清单
- `examples/` 提供输入与输出示例
- 重要提醒：当 AI 真正启用这个 skill 后，通常会显著增加 token 消耗，尤其是在多 node-id、多画板、多断点、多状态稿场景下
- 若你希望 AI 先生成 `create.md` 级详细文档，再继续写代码，优先直接使用 `references/create-doc-prompt-template.md`
- 若项目已经引入全局 CSS、UI 框架、自定义主题包、断点容器或栅格系统，建议在输入时一并补齐，让 AI 先按项目实现基线判断宽度与内部间距策略

## 推荐触发方式

```text
请使用 figma-spec-to-ui-1to1。
目标是高保真、接近 1:1 还原。
请先输出结构化 UI 设计说明书，再输出开发方案，最后生成代码。
```
