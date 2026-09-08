> ⚠️ **已整合 / 不再单独维护（2026-09-08）**
> 本技能的独有能力（会话断点恢复、复杂度裁剪、九专家角色卡）已并入唯一全栈总控 **`fullstack-delivery-orchestrator`**，此处仅作历史备份保留，不再单独调用。
> - 全栈端到端 / 跨层开发 / 架构 / Agent：请用 `fullstack-delivery-orchestrator`
> - 单点小 bug 快速修复：请用 `bugfix-fast`
> - 整合后技能见 https://github.com/lqh1314/fullstack-delivery-skills 的 `skills/fullstack-delivery-orchestrator/`

# fullstack-flow — 全栈项目端到端交付流水线 Skill

一个单入口 Skill，把全栈功能从「模糊想法」带到「可上线交付」：
**需求业务澄清 → 数据与架构设计 → 分层实现 → 测试/安全/性能三路评审 → 部署与文档交接**。
内置三个用户审批闸门、阶段产出落盘、失败即停与根因调试，支持任务复杂度自动裁剪。

## 适用 / 不适用

- 适用：全栈应用、端到端新功能、前端+后端+数据库两层以上联动、从零搭项目；
- 不适用：单文件小修、纯 bug 修复、纯问答、只改样式（这些走单点工具更快）。

## 安装

1. 获取本目录（解压 zip 或 clone 仓库），确保目录结构为：

```
fullstack-flow/
├── SKILL.md
└── references/
    ├── 01-requirements.md
    ├── 02-design.md
    ├── 03-implementation.md
    ├── 04-testing.md
    ├── 05-delivery.md
    └── role-cards.md
```

2. 把整个 `fullstack-flow/` 目录放到你所用 Agent 的 Skill 根目录（任选其一）：
   - 豆包 / Claude Code 类环境：`~/.claude/skills/` 或运行时的 `skills/`、`.user_skills/` 目录；
   - 其他支持 Agent Skills 的 harness：其文档指定的 skills 目录。
3. 重启或刷新会话，让 Skill 被扫描到。

## 使用

- 显式调用：`用 fullstack-flow 做一个 XXX（功能描述）`
- 自动触发：描述一个涉及前后端+数据库的端到端需求即可自动启用。
- 裁剪标志：
  - `--simple`：小功能轻流程；
  - `--no-frontend`：纯后端 API；
  - `--no-db`：无持久化；
  - `--complex`：新项目/大功能完整仪式。

## 流程与闸门

| 阶段 | 产出（项目内 `.fullstack-flow/`） | 闸门 |
|---|---|---|
| 1 需求与业务 | 01-requirements.md | 闸门 A：需求确认 |
| 2 设计 | 02-database-design.md、03-architecture.md | 闸门 B：架构评审 |
| 3 实现（DB→后端→前端） | 04/05/06 实现摘要 | — |
| 4 验证（测试+安全+性能并行） | 07-testing.md | 闸门 C：测试评审 |
| 5 交付 | 08-deployment.md、09-documentation.md | 完成定义核验 |

过程文件全部落盘，中断后再次调用可从断点恢复。

## 方法论来源

整合自 wshobson 全栈九阶段编排、superpowers（brainstorming / TDD / verification / systematic-debugging）工程纪律、gstack 的业务拷问与 QA 回归方法；若本地已安装 ws-* 专家 Skill，会自动复用。
