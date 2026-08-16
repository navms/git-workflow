# git-workflow

一个给 DSH（DeepSeek Harness）编码会话使用的 Agent Skill：把「分支开发 → 推送确认 → 合并回主干」变成一套显式的工作流，关键动作（创建分支、推送、合并）必须经用户确认。

## 解决的痛点

DSH 中的编码 Agent 完成一个需求后，往往会自行 `git add` / `git commit`，改动直接落在当前分支（通常是 master/main）上：

- **没有审查环节**：提交发生时用户还没来得及看 diff；
- **改动直接进主干**：主干被半成品、未审查的提交污染，本应「随时可发布」的状态被破坏。

本 skill 在工作流中插入**三个必须询问用户的决策点**：

1. 检测到新需求、动手改代码**之前** → 询问是否创建新分支；
2. 任何 `git push`（特性分支、合并后的主干）**之前** → 询问是否推送；
3. 开发完成**之后** → 询问是否合并回主干。

**未经用户确认，Agent 不得在主干上修改或提交代码，也不得推送到远程。**

## 安装

```bash
mkdir -p ~/.dsh/skills
cp -r git-workflow ~/.dsh/skills/
```

## 使用

- 任何带 skill 工具的 DSH 会话（标准 / 创造等预设）都会自动发现该 skill，无需额外配置；
- 用户提出需要修改代码的新需求，或提到分支、合并、提交、PR 等 git 概念时，Agent 会加载并遵循本工作流；
- 极简模式（minimal）没有 skill 工具，无法使用；
- skill 是通用格式（`SKILL.md` + `name`/`description` frontmatter），也可以移植到 Claude Code（`~/.claude/skills/`）、Cursor（`.cursor/skills/`）等支持 Agent Skills 的客户端，注意各客户端工具名差异即可。

## 仓库结构

```
git-workflow/
├── git-workflow/SKILL.md        # skill 本体
```

## License

MIT
