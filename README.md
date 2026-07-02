# Logos

> AI 对话沉淀与日报生成：把 AI Coding 对话整理成可追溯的工作日志。

## 这是什么

Logos 是一个通用 AI Skill，面向 **AI Coding 工作流**。当你在对话末尾手动触发，或通过宿主工具的 hook / automation 触发时，AI 会回顾本轮对话，将需求、决策、产物、待办和经验沉淀到按日期组织的 Markdown 日报中。

默认输出路径：

```text
docs/daily/YYYY-MM-DD.md
```

Logos 当前版本是 **Skill 指令包**，不是独立 CLI。它依赖 Claude Code、Codex、Cursor、CodeBuddy 等宿主工具读取当前对话上下文并执行写文件操作。

## 解决的问题

AI 编程对话里经常包含需求分析、技术取舍、临时问题、代码产物和经验教训。默认情况下，这些上下文会分散在聊天记录里，第二天很难快速回忆：

- 昨天为什么选这个方案？
- 哪些文件被改过？
- 哪些坑已经踩过？
- 哪些待办还没收尾？

Logos 的目标是把每一次有价值的 AI 对话变成可搜索、可追加、可回溯的项目知识档案。

## 安装

把仓库放到你的 AI 工具可识别的 skills 目录中：

```bash
git clone https://github.com/myyyyyyz/logos.git skills/logos/
```

如果你的工具使用不同的 skills 目录，请将 `logos/` 放到对应目录下，并确认 `SKILL.md` 位于 skill 根目录。

## 使用方式

### 手动触发

在一轮有实质内容的对话末尾说：

- `记录本次对话`
- `总结对话`
- `写日报`
- `logos`

AI 应按 `SKILL.md` 中的流程读取当前上下文，并写入 `docs/daily/YYYY-MM-DD.md`。

### 自动触发

自动触发需要宿主工具支持 hook、定时任务或 automation。Logos 只提供 Skill 规则，不负责安装宿主工具 hook。

Claude Code 风格示例：

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "command": "请执行 logos skill，总结本轮有实质内容的 AI Coding 对话"
      }
    ]
  }
}
```

其他工具建议：

- Cursor / CodeBuddy：使用内置规则、定时任务或 automation，在会话结束后提示执行 `logos`。
- Codex：在需要沉淀时手动说 `logos`，或使用平台支持的线程/任务自动化能力触发。
- 不支持自动 hook 的工具：使用手动触发即可。

## 日报格式

每篇日报包含：

| 区块 | 内容 |
|------|------|
| 需求概述 | 本轮对话的核心目标 |
| 关键决策 | 技术选型、方案取舍及原因 |
| 对话摘要 | 3-5 句话概括全过程 |
| 产物清单 | 新建、修改、删除了哪些文件 |
| 待办 / 遗留 | 未完成事项和后续动作 |
| 经验沉淀 | 值得复用的经验和踩坑记录 |

示例见：

```text
examples/daily/2026-07-02.md
```

## 工作流

```text
回顾对话 -> 生成结构化摘要 -> 脱敏与去重检查 -> 写入 docs/daily/YYYY-MM-DD.md
```

写入规则：

- 当日文件不存在：创建文件头，再写入第一段对话摘要。
- 当日文件已存在：追加到文件末尾，段间用 `---` 分隔。
- 同日多段对话使用 `第 N 次对话` 标题，`N` 从已有日报中最大的编号继续递增。
- 如果当前上下文不足以准确总结，应在待办中明确“上下文不足”，不得编造细节。
- 如果只是闲聊或测试，不应强制写日报。

## 隐私与安全

Logos 会沉淀对话上下文，可能包含敏感信息。写入前应执行最小脱敏：

- 不记录 API Key、Token、Cookie、密码、私钥。
- 不记录客户隐私、个人身份证明信息或商业敏感原文。
- 对敏感路径、账号、域名可用 `[REDACTED]` 代替。
- 如果不确定某段内容是否可写入，应在待办里提示人工确认。

## 验证

本仓库提供一个零依赖 PowerShell 验证脚本，用于检查 Skill 包结构：

```powershell
powershell -ExecutionPolicy Bypass -File scripts/validate.ps1
```

验证内容包括：

- 必需文件是否存在。
- `SKILL.md` YAML frontmatter 是否闭合。
- `references/template.md` 是否存在。
- README 是否包含安装、触发、隐私和验证说明。
- 示例日报是否存在。

## 当前限制

- Logos 不是独立 CLI，不能自己读取不同工具的会话数据库。
- 自动触发依赖宿主工具能力，不同工具需要分别配置。
- 摘要质量依赖当前上下文窗口；上下文被压缩或截断时可能只能生成不完整记录。
- 并发写入同一个日报文件时没有锁机制，建议避免多个 agent 同时写同一天文件。

## 文件结构

```text
logos/
├── SKILL.md
├── README.md
├── references/
│   └── template.md
├── examples/
│   └── daily/
│       └── 2026-07-02.md
└── scripts/
    └── validate.ps1
```
