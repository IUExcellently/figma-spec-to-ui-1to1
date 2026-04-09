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

## 推荐触发方式

```text
请使用 figma-spec-to-ui-1to1。
目标是高保真、接近 1:1 还原。
请先输出结构化 UI 设计说明书，再输出开发方案，最后生成代码。
```
