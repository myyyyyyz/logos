# Logos

> AI 对话沉淀与日报生成 — 每次对话自动记录，AI 智能总结，知识永不丢失。

## 这是什么

Logos 是一个通用 AI Skill，专为 **AI Coding 工作流** 设计。每次 AI 对话结束后，自动拉取对话内容，生成结构化摘要并沉淀为按天组织的日报文件。

支持 Claude Code、Cursor、Codex、CodeBuddy 等任意支持 Skill 机制的 AI 编程工具。

## 解决的问题

用 AI 写代码时，每一轮对话都包含了需求分析、技术决策、代码产物和经验教训。但默认情况下，这些对话"聊完就丢"——第二天你可能已经不记得昨天做了什么决策、为什么这样做。

Logos 把每一次对话转化为可回溯的知识档案。

## 安装

```bash
git clone https://github.com/myyyyyyz/logos.git skills/logos/
```

## 使用方式

### 手动触发

对话末尾说：
- "记录本次对话"
- "总结对话"
- "logos"

### 自动触发（推荐）

通过 AI 工具的定时调度或 Hook 机制配置自动执行。以每日 19:00 为例：

- **Claude Code**: 配置 SessionEnd hook 调用本 skill
- **CodeBuddy / Cursor**: 通过内置的定时任务功能，配置 `执行 logos skill` 即可
- 其他工具: 参考对应平台的 hook / automation 文档

## 产出的日报长什么样

```
docs/daily/
└── 2026-07-01.md
```

每篇日报包含：

| 区块 | 内容 |
|------|------|
| 📋 需求概述 | 一轮对话的核心目标 |
| 🎯 关键决策 | 技术选型、方案取舍及原因 |
| 📝 对话摘要 | 3-5 句话概括全过程 |
| 📦 产物清单 | 新建/修改/删除了哪些文件 |
| ⚠️ 待办 | 遗留问题 |
| 💡 经验沉淀 | 值得记住的经验、踩过的坑 |

## 工作流

```
回顾对话 → AI 生成摘要 → 写入 docs/daily/YYYY-MM-DD.md
```

- 同日多段对话追加到同一文件，用 `---` 分隔
- 不覆盖已有内容，始终追加

## 文件结构

```
logos/
├── SKILL.md                  # Skill 主文件
└── references/
    └── template.md           # 日报摘要模板
```
