# figma-spec-to-ui-1to1 for Codex 安装说明

本仓库同时支持：

- Codex：通过原生 skill 发现机制加载 `SKILL.md`
- Kiro：通过 `POWER.md` 作为 custom power 安装

如果你正在 Codex 中执行本安装，请按以下步骤完成。

## 快速安装

1. 克隆仓库到本地：

```bash
git clone https://github.com/IUExcellently/figma-spec-to-ui-1to1.git ~/.codex/figma-spec-to-ui-1to1
```

2. 创建 Codex skills 目录：

```bash
mkdir -p ~/.agents/skills
```

3. 将整个仓库链接到 Codex skills 目录：

```bash
ln -s ~/.codex/figma-spec-to-ui-1to1 ~/.agents/skills/figma-spec-to-ui-1to1
```

4. 重启 Codex，或重开当前会话。

## 手动验证

确认以下文件存在：

```bash
ls ~/.agents/skills/figma-spec-to-ui-1to1
```

至少应能看到：

- `SKILL.md`
- `README.md`
- `references/`
- `examples/`

## 使用方式

安装完成后，在 Codex 中直接提及 skill 名称即可，例如：

```text
请使用 figma-spec-to-ui-1to1。
我会提供 Figma 链接、node-id、页面结构和补充规格。
请先输出结构化 UI 设计说明书，再输出开发方案，最后再生成代码。
```

## 更新

```bash
cd ~/.codex/figma-spec-to-ui-1to1
git pull
```

## 卸载

```bash
rm ~/.agents/skills/figma-spec-to-ui-1to1
rm -rf ~/.codex/figma-spec-to-ui-1to1
```
