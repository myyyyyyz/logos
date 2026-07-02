# Logos

> 把每一次 AI Coding 对话沉淀为可追溯的工作日报。

## 快速开始

### 安装

```bash
git clone https://github.com/myyyyyyz/logos.git skills/logos/
```

> 放到你的 AI 工具的 skills 目录即可，不同工具的路径见下表。

### 立即使用

在任意一轮有实质内容的 AI 对话末尾说：

> **"记录本次对话"** 或 **"logos"**

AI 会自动回顾本轮对话，生成结构化摘要，写入 `docs/daily/YYYY-MM-DD.md`。

### 各平台安装 + 触发速查

| 工具 | 安装路径 | 手动触发 | 自动触发 |
|------|---------|---------|---------|
| **Claude Code** | `.claude/skills/logos/` 或 `/plugin install` | 说 `logos` | 配置 Stop hook |
| **Codex CLI** | `.codex/skills/logos/` | 说 `logos` | 配置 automation |
| **Cursor** | `.cursor/skills/logos/` | 说 `logos` | `/add-plugin` 或规则触发 |
| **CodeBuddy** | `.codebuddy/skills/logos/` | 说 `logos` | 配置定时任务 |
| **Antigravity** | 项目 skills 目录 | 说 `logos` | 配置 hook |
| **OpenCode** | `skills/logos/` | 说 `logos` | 配置 hook |

### 30 秒看效果

```text
docs/daily/2026-07-02.md

# 2026年07月02日 工作日报
> 自动生成于 2026-07-02 19:00 | 由 Logos 驱动

---

### 18:20 ~ 18:55 | 第 1 次对话

#### 需求概述
{一句话描述}

#### 关键决策
| 决策 | 原因 |
|------|------|
| ... | ... |

#### 对话摘要
{3-5 句话概括}

#### 产物清单
- `新建` `file.py` - ...
- `修改` `config.json` - ...

#### 待办 / 遗留
- [ ] ...

#### 经验沉淀
...
```

完整示例：[examples/daily/2026-07-02.md](examples/daily/2026-07-02.md)

---

## 这是什么

Logos 是一个通用 AI Skill。当你在对话中说 **"logos"** 或 **"记录本次对话"**，AI 会回顾本轮上下文，将需求、决策、产物、待办和经验整理成结构化日报，写到 `docs/daily/YYYY-MM-DD.md`。

它不是 CLI 工具，而是告诉 AI "该怎么总结对话"的指令包。

## 解决的问题

AI 编程对话里包含需求分析、技术取舍、产物和经验。但默认情况下聊完就丢，第二天很难回忆：
- 昨天为什么选了那个方案？
- 哪些文件被改过？
- 哪些待办还没收尾？

Logos 把每一次有价值的对话变成可搜索、可追加的知识档案。

## 日报内容

| 区块 | 内容 |
|------|------|
| 需求概述 | 本轮对话的核心目标 |
| 关键决策 | 技术选型、方案取舍及原因 |
| 对话摘要 | 3-5 句话概括全过程 |
| 产物清单 | 新建、修改、删除了哪些文件 |
| 验证结果 | 命令执行情况 |
| 待办 / 遗留 | 未完成事项 |
| 经验沉淀 | 值得复用的经验和踩坑 |

同日多段对话追加到同一文件，段间用 `---` 分隔，自动递增 `第 N 次对话` 编号。

## 自动触发

手动说 "logos" 就能用。如果想每次对话结束自动记录，配置 Hook：

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

其他工具（Cursor、CodeBuddy 等）使用各自的定时任务或 automation 能力触发即可。

## 隐私与安全

Logos 沉淀对话内容，写入前自动脱敏：
- 不记录 API Key、Token、密码、私钥
- 敏感信息用 `[REDACTED]` 替代
- 不确定时标记为待人工确认

## 验证

```powershell
powershell -ExecutionPolicy Bypass -File scripts/validate.ps1
```

检查 Skill 包结构完整性。

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

## 当前限制

- 不是独立 CLI，依赖宿主 AI 工具读取上下文
- 自动触发依赖宿主工具的 hook / 定时任务能力
- 摘要质量依赖上下文窗口；被压缩时可能不完整
- 无并发锁，避免多个 agent 同时写同一天文件

