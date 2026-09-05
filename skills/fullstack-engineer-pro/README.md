# FullStack Engineer Pro · 资深全栈工程师总控 Skill

> 一个 Skill，把"资深全栈工程师接到任务后的完整工作方式"固化下来：**需求 → 架构 → 分层实现 → 测试/安全/性能三路评审 → 部署交付**全生命周期编排，内置 **Python / Java·Spring·Quarkus / JavaScript·TypeScript·React·Node** 多语言工程手册，覆盖 **Agent/LLM 应用开发、系统架构、Bug 根因定位与深度修复、TDD 与生产可观测**。
>
> 面向懂 Agent 开发、需要在多语言与多场景间快速切换、要求"快速且靠谱地解决问题"的工程师。兼容 [Claude Code Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) / 兼容 Anthropic Agent Skills 规范（`SKILL.md` + 渐进加载 `references/`）。

---

## 它解决什么问题

单个技能只覆盖一个点，真实开发却需要一条完整链路：需求没问清就写代码、架构边界被穿透、bug 只修症状、Java/Spring 代码被 AI 写出大量"看起来对其实错"的幻觉（字段注入、事务自调用失效、N+1、H2 冒充生产库……）、写完不验证就宣称完成。

本 Skill 用**单入口总控**解决：先分诊任务类型 → 选对主线流程 → 在正确阶段加载正确的语言手册与本地专家技能 → 用审批闸门和质量门守住结果。它是**编排器与质量门**，不是又一堆零散提示词。

## 核心能力地图

| 能力 | 说明 | 入口 |
|---|---|---|
| **四条工作主线** | A 端到端全栈交付 / B Bug 根因调试 / C 架构设计 / D Agent 开发 | `SKILL.md` 场景路由 |
| **全生命周期流水线** | 五阶段 + 三个审批闸门 + 状态落盘 + 失败即停 + 完成定义(DoD) | `references/01` |
| **Bug 精准定位深度修复** | Stop-the-line 六步分诊、git bisect、分语言报错决策树、JVM 调试、3 次失败重假设铁律 | `references/02` |
| **架构逻辑** | 分层 / 六边形(端口-适配器) / DDD、API 契约、事务与 Saga 一致性、微服务韧性、ADR | `references/03` |
| **Java / Spring / JVM（自包含重点）** | Spring Boot 3/4、Spring Data JPA、事务、测试金字塔、Spring Security、Resilience4j、Flyway、多模块 Maven、Quarkus、GraalVM、JVM 性能、Spring AI | `references/04` |
| **Python 工程** | FastAPI、async、错误处理、性能要点 + 本地 15+ python 技能路由 | `references/05` |
| **Web / Node / TS** | React 状态与三态、Node 后端、TS、可访问性 + 本地前端技能路由 | `references/06` |
| **Agent / LLM 开发** | Agent 模式选型、LangChain/LangGraph、RAG、MCP、多智能体、评估与生产化清单 | `references/07` |
| **质量门** | TDD、五轴代码评审、OWASP 安全、性能、RED 可观测、发布检查单 | `references/08` |
| **专家角色与业务流程** | 9 类专家角色卡、SDLC 全流程映射、复杂度裁剪、多线协作 | `references/09` |
| **本地技能路由** | 237 个本地技能的"主题→技能"索引 + 全部来源致谢 | `references/10` |

## 三条最重要的设计

1. **闸门处必须停**：需求、架构、验证结果三处必须等你确认再继续——跑偏成本随阶段指数上升，越早拦越便宜。
2. **失败即停，改根因不改症状**：任何报错立即进入六步根因调试，禁止注释报错、空 catch、带着失败继续堆代码；每个修复必须配一个"无修复就会失败"的回归测试。
3. **入乡随俗 + 证据优先**：先读项目里最相似的现有实现再写代码；"看起来对"不算完成，必须在真实入口运行验证，Critical/High 不清零不交付。

## 目录结构

```
fullstack-engineer-pro/
├── SKILL.md                          # 总入口：能力声明、场景路由、四主线、七原则、DoD
├── README.md                         # 本文件
├── LICENSE                           # MIT
└── references/                       # 渐进加载分册（按需读，不一次性占上下文）
    ├── 01-intake-and-lifecycle.md    # 任务分诊 + 五阶段落盘模板
    ├── 02-root-cause-debugging.md    # 六步根因调试 + 分语言分诊 + JVM 调试
    ├── 03-architecture-playbook.md   # 分层/六边形/DDD + 契约 + 事务一致性 + 微服务 + ADR
    ├── 04-java-spring-playbook.md    # ★ Java/Spring/Quarkus/JVM 自包含手册
    ├── 05-python-playbook.md         # Python 要点 + 本地技能路由
    ├── 06-web-node-playbook.md       # 前端/Node/TS 要点 + 本地技能路由
    ├── 07-agent-dev-playbook.md      # Agent/LangChain/MCP/RAG/多智能体
    ├── 08-quality-gates.md           # TDD/评审/安全/性能/可观测/发布
    ├── 09-business-workflows.md      # 专家角色卡 + SDLC 业务流程
    └── 10-skill-router.md            # 本地技能路由索引 + 来源致谢
```

## 安装

### 方式一：放到技能目录（推荐）
```bash
# 克隆到你的 Agent 技能根目录（Claude Code 为 ~/.claude/skills，本环境为 .user_skills/）
git clone https://github.com/lqh1314/fullstack-engineer-pro.git \
  ~/.claude/skills/fullstack-engineer-pro
```
重启 Agent 后，当你说"走全流程 / 端到端做一个 / 深度排查这个 bug / 写个 Spring 服务 / 开发一个 Agent"等，会自动加载 `SKILL.md`，再按场景按需读取 `references/`。

### 方式二：仅当知识库阅读
直接按目录阅读对应分册即可，每册独立成文，可单独用于代码评审对照、面试准备、团队规范。

## 怎么用（典型场景）

- **从零做一个功能**："用全栈流程做一个订单服务（Java + Postgres + React）" → 主线 A，自动走五阶段与三闸门，进入后端时加载 04。
- **修一个难缠的 bug**："这个接口偶发 500，深度排查" → 主线 B，六步分诊，先复现再根因修复并补回归测试。
- **评审/设计架构**："帮我评审这个微服务拆分方案" → 主线 C + 03 + 08，输出问题清单与 ADR。
- **写 AI Agent**："做一个能查订单库的 RAG 助手" → 主线 D + 07，先选模式再定组件，按生产化清单交付。
- **纯 Java/Spring 任务**：任何 Spring 代码生成/排错都会加载 04，逐条对照"AI 高频幻觉红线"。

可用标志裁剪仪式：`--simple`（轻量）、`--complex`（完整）、`--no-frontend`、`--no-db`；复杂度拿不准时默认走更重的路径。

## 与其他技能的关系

本 Skill **编排而不重复**：运行时若本地已安装对应专家技能（如 `ws-backend-architect`、`python-*`、`test-driven-development`、`langchain-architecture`），会先读取复用；唯一**自包含**的是 Java/JVM 手册（04），因为多数通用技能库在这块是空白。完整路由见 `references/10`。

## 来源与致谢（均为 MIT 许可）

本技能是对以下优秀开源成果的**提炼、翻译与编排整合**（非逐字搬运）：

**方法论框架**
- [obra/superpowers](https://github.com/obra/superpowers) — 头脑风暴、计划、TDD、根因调试、子智能体
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — 六阶段工程流、Stop-the-line 调试、五轴评审、可观测

**角色与技能库**
- [wshobson/agents](https://github.com/wshobson/agents) — 全栈角色、调试工具集、Python 技能
- [Jeffallan/claude-skills](https://github.com/Jeffallan/claude-skills) — 功能开发与调试工作流
- [anthropics/skills](https://github.com/anthropics/skills) — MCP/技能构建与测试方法论

**Java / JVM 专项（04 分册主要事实来源）**
- [amplicode/spring-skills](https://github.com/amplicode/spring-skills) — Spring 工具厂商官方，结构化工程上下文、IDE Debug MCP
- [rrezartprebreza/spring-boot-skills](https://github.com/rrezartprebreza/spring-boot-skills) — Spring Boot 3/4 生产级规则与"反 AI 幻觉"Gotchas
- [wallaceespindola/ai-agents](https://github.com/wallaceespindola/ai-agents) — 企业级 Spring Cloud/Quarkus/JVM 技能

## 许可证

[MIT](./LICENSE)。各上游源仓库保留其各自许可与版权。
