# Agent Memory Framework

> **比 CLAUDE.md 更有效的 AI Agent 上下文记忆方案**

## 痛点

使用 AI Coding Agent（如 Claude Code、GitHub Copilot、Cursor 等）时，CLAUDE.md / .cursorrules 等方案存在局限：

| 问题 | CLAUDE.md | 本方案 |
|------|-----------|--------|
| 单文件过长，读取效率低 | ❌ 一个大文件 | ✅ 4 个分层文件，按需读取 |
| 没有实时进度追踪 | ❌ 手动维护 | ✅ current-status.md 自动更新 |
| 不区分信息优先级 | ❌ 全部平铺 | ✅ 项目身份 / 规范 / 进度 / 流程分层 |
| 缺乏防错机制 | ❌ 无 | ✅ dev-checklist + 历史教训 |
| 用户偏好和代码规范混在一起 | ❌ 混杂 | ✅ context.md vs conventions.md 分离 |

## 目录结构

```
.agents/
├── context.md              ← 项目身份证 (每次新对话必读)
│   ├── 项目基本信息（技术栈/仓库/端口）
│   ├── 架构核心概要
│   └── 用户偏好 ⚠️
│
├── conventions.md          ← 代码规范
│   ├── 后端规范（命名/目录/异步）
│   ├── 前端规范
│   └── Git 约定
│
└── workflows/
    ├── current-status.md   ← 实时进度 (Living Document)
    │   ├── 正在做
    │   ├── 最近完成
    │   ├── 已知问题
    │   ├── 待做队列
    │   └── 历史教训 ⚠️ (防止重复犯错)
    │
    └── dev-checklist.md    ← 开发检查清单
        ├── 新功能开发步骤
        └── 常见遗漏提醒
```

## 设计原则

### 1. 分层读取，按需加载

不是所有信息每次都需要。Agent 的读取策略：

| 文件 | 读取时机 | 信息类型 |
|------|---------|---------|
| `context.md` | **每次新对话必读** | 项目是什么、关键约定 |
| `current-status.md` | **继续工作时读** | 现在做到哪了、有什么坑 |
| `conventions.md` | **写代码时读** | 怎么命名、怎么提交 |
| `dev-checklist.md` | **开发新功能时读** | 步骤清单、防遗漏 |

### 2. 活文档，自动更新

`current-status.md` 不是写完就不管的 — Agent 在每次完成任务后应主动更新：
- 把"正在做"移到"最近完成"
- 把新发现的问题加入"已知问题"
- 把犯过的错误加入"历史教训"

### 3. 防犯错机制

`current-status.md` 中的"历史教训"是关键创新：

```markdown
## 历史教训 (⚠️ 别再犯)
- 写完工具必须立即接入主 Agent (不要延迟集成)
- 开发多轮对话功能后必须测试上下文连续性
- 先写测试 case 再开发，不要反过来
```

这些经验会在每次新对话时被读取，避免 Agent 重复犯相同错误。

## 快速开始

### 1. 复制模板

```bash
mkdir -p .agents/workflows
cp templates/context.md .agents/
cp templates/conventions.md .agents/
cp templates/current-status.md .agents/workflows/
cp templates/dev-checklist.md .agents/workflows/
```

### 2. 根据你的项目修改

- `context.md`: 填入你的项目信息、技术栈、架构
- `conventions.md`: 填入你的代码规范
- `current-status.md`: 填入当前进度
- `dev-checklist.md`: 填入你的开发流程

### 3. 告诉 Agent 读取

在对话开始时，Agent 会自动扫描 `.agents/` 目录。或者你可以提示：

> "请先读取 .agents/ 目录了解项目上下文"

## 适用场景

- ✅ 任何使用 AI Coding Agent 的项目
- ✅ 需要跨多次对话保持上下文的长期项目
- ✅ 多人协作时统一 Agent 行为
- ✅ 替代或增强 CLAUDE.md / .cursorrules

## 与 CLAUDE.md 对比

```
CLAUDE.md 方式:
┌────────────────────────────────┐
│ 项目信息 + 代码规范 + 命令 +  │
│ 偏好 + 约定 + ... 全在一个    │
│ 大文件里，每次全部读取        │
└────────────────────────────────┘

Agent Memory Framework 方式:
┌──────────┐  ┌───────────────┐  ┌────────────────┐
│ context  │  │ conventions   │  │ workflows/     │
│ (必读)   │  │ (写码时读)    │  │ ├ status (活)  │
│ 20行     │  │ 25行          │  │ └ checklist    │
└──────────┘  └───────────────┘  └────────────────┘
按需加载，总量更小，更新更频繁
```

## License

MIT
