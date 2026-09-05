# Fullstack Delivery Skills

一套面向 AI 编程助手的「全栈交付」技能合集：把"从 0 搭建系统 → 架构设计 → 分层实现 → 测试/安全/性能质量门 → 交付验证"固化为带**阶段质量门**和**技能精确路由**的作业流程，覆盖需求澄清、深度排障、Java/Spring、Python/FastAPI、前端、数据库、Agent/MCP 开发与项目管理。

## 包含的三个技能

| 技能 | 定位 |
|---|---|
| `fullstack-delivery-orchestrator` | **总控/编排层（本合集原创）**：三问分诊（新建 / 排障 / 接手）+ S1–S6 六阶段质量门 + 11 大能力域技能路由表；附带可执行的交付前静态体检脚本 `scripts/preflight_check.py`（纯标准库，抓空吞异常、硬编码密钥、调试断点、被跳过的测试等）。 |
| `fullstack-engineer-pro` | 资深全栈工程师单入口总控，内置 Java/Spring、Python、Web/Node 多语言手册与 Agent 开发专项。 |
| `fullstack-flow` | 端到端全栈交付流水线（需求→设计→实现→三路评审→交付），含审批闸门、状态落盘、失败即停。 |

> 后两个技能用于与总控对照、互为补充；三者可按任务复杂度择一为主线，`orchestrator` 的 `skill-map.md` 负责在正确阶段把工作分派给正确的专门技能。

## 目录结构

```
fullstack-delivery-skills/
└── skills/
    ├── fullstack-delivery-orchestrator/   # 原创总控
    │   ├── SKILL.md
    │   ├── references/ (5 份：路由表/质量门/分栈手册/排障SOP/完整走查例题)
    │   └── scripts/preflight_check.py
    ├── fullstack-engineer-pro/            # SKILL.md + references/ (10 份)，自带 LICENSE
    └── fullstack-flow/                    # SKILL.md + references/ (6 份)
```

## 安装

把 `skills/` 下需要的技能文件夹整个复制到你的 AI 助手技能目录（例如本环境的 `~/.super_doubao/.../workspace/.user_skills/`），保持每个技能目录内 `SKILL.md` 位于该技能文件夹根部即可，新开会话自动加载。

仅使用总控时，最小安装为 `fullstack-delivery-orchestrator/` 一个目录；它会按需路由到你本地已安装的其他专门技能。

体检脚本可独立运行：

```bash
python3 skills/fullstack-delivery-orchestrator/scripts/preflight_check.py <项目根> --stage s6
# 退出码：0 无 BLOCKER；1 存在 BLOCKER；2 路径/用法错误
```

## 来源与许可

- `fullstack-delivery-orchestrator`：在本环境中原创整合，可自由使用、修改、分发。
- `fullstack-engineer-pro`、`fullstack-flow`：整理自公开开源的 AI 编程技能社区（其方法学源自 wshobson/agents、obra/superpowers、addyosmani/agent-skills、amplicode/spring-skills 等，具体见各技能目录内自带的 `LICENSE` / `README` 来源致谢），本合集仅作收录与对照，相应权利归原作者所有，请遵循其各自许可证。
