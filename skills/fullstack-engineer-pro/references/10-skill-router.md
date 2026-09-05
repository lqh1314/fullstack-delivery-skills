# 10 · 本地技能路由索引与来源致谢

> 本技能是**总控与编排器**：专业细节优先复用本地已装技能，本文件给出"遇到什么主题 → 读哪个本地技能"的路由。若同名技能不存在，用本技能 references 里的自包含内容兜底。技能名以本机 `.user_skills/` 实际安装为准（下表基于 237 个本地技能盘点）。

## 1. 全栈角色与编排（ws-*）
| 需求 | 本地技能 |
|---|---|
| 端到端全栈特性（本技能的前身/锚点） | `fullstack-flow`、`ws-full-stack-feature` |
| 后端方案/实现 | `ws-backend-architect`、`ws-backend-feature-dev` |
| 前端实现/设计系统/组件 | `ws-frontend-developer`、`ws-design-system-architect`、`ws-component-scaffold` |
| 数据库/SQL | `ws-database-architect`、`ws-sql-pro` |
| GraphQL | `ws-graphql-architect` |
| 移动端 | `ws-mobile-developer`、`react-native-architecture`、`react-native-design` |
| TDD/测试自动化 | `ws-tdd-orchestrator`、`ws-test-automator` |
| 安全/性能/部署 | `ws-security-auditor`、`ws-performance-engineer`、`ws-deployment-engineer` |
| 架构图/评审/事件溯源/C4 | `ws-c4-architect`、`ws-architect-review`、`ws-event-sourcing-architect` |
| 多轨编排 | `ws-conductor-setup/new-track/implement` |

## 2. 调试与质量
| 主题 | 本地技能 |
|---|---|
| 系统化调试方法论 | `debugging-strategies`、`parallel-debugging`（另：本技能 02 已自包含六步法） |
| TDD / 完成前验证 | `test-driven-development`、`verification-before-completion` |
| 代码评审 | `code-review-excellence`、`requesting-code-review`、`receiving-code-review`、`multi-reviewer-patterns` |
| 各语言测试 | `python-testing-patterns`、`javascript-testing-patterns`、`e2e-testing-patterns`、`webapp-testing`、`temporal-python-testing` |
| 质量门禁/数据质量 | `pptx-quality-gates`(演示类)、`data-quality-frameworks` |

## 3. 架构 / 分布式 / 云与 DevOps
`architecture-patterns`、`architecture-decision-records`、`api-design-principles`、`microservices-patterns`、`saga-orchestration`、`cqrs-implementation`、`event-store-design`、`distributed-tracing`、`service-mesh-observability`、`multi-cloud-architecture`、`hybrid-cloud-networking`、`deployment-pipeline-design`、`gitlab-ci-patterns`、`k8s-manifest-generator`、`k8s-security-policies`、`terraform-module-library`、`incident-runbook-templates`。

## 4. Python（另见 05）
`fastapi-templates`、`async-python-patterns`、`python-{anti-patterns,background-jobs,code-style,configuration,design-patterns,error-handling,observability,packaging,performance-optimization,project-structure,resilience,resource-management,testing-patterns,type-safety}`。

## 5. 前端 / Node / TS（另见 06）
`frontend-design`、`react-{modernization,state-management,native-architecture,native-design}`、`nextjs-app-router-patterns`、`nodejs-backend-patterns`、`typescript-advanced-types`、`modern-javascript-patterns`、`responsive-design`、`interaction-design`、`web-component-design`、`accessibility-compliance`、`auth-implementation-patterns`、`session-guard`、`secrets-management`。

## 6. Agent / LLM 应用（另见 07）
| 主题 | 本地技能 |
|---|---|
| LangChain/LangGraph 架构 | `langchain-architecture` |
| RAG 检索增强 | `rag-implementation` |
| 构建 MCP 服务 / MCP 安全 | `mcp-builder`、`protect-mcp-setup` |
| 多智能体/并行/长任务 | `subagent-driven-development`、`dispatching-parallel-agents`、`review-agent-setup` |
| 提示工程 / 评估 / 推荐管线 | `prompt-engineering-patterns`、`llm-evaluation`、`recsys-pipeline-architect` |

## 7. 工程方法论（superpowers 系）
`brainstorming`（发散需求）、`writing-plans`（细粒度计划）、`executing-plans`（按计划执行）、`using-git-worktrees`（隔离开发）、`finishing-a-development-branch`（合并前检查）、`using-superpowers`。

## 8. Java / JVM（本地空白 → 用本技能 04 自包含手册）
本地技能库基本没有 Java/JVM 深度技能（`jvm-languages` 类内容以"人设 agent"为主、工程规则很少）。因此 **Java/Spring Boot/Spring Cloud/Quarkus/JVM 一律以本技能 `references/04-java-spring-playbook.md` 为准**，它已自包含分层、REST、JPA、事务、测试、安全、韧性、迁移、多模块、Quarkus、性能、Spring AI 全链路。

## 9. 使用规则
1. 进入某专业判断前，先在本表查本地技能，**读其 SKILL.md 后按其方法执行**；本技能不重复搬运其内容。
2. 本地技能与本手册冲突时：项目既有约定 > 用户决策 > 两者择优，并向用户说明分歧。
3. 本地技能缺失时用本技能 references 兜底；仍缺信息时如实说明，不编造 API/注解/版本特性。

## 10. 外部来源与致谢（本技能整合自以下开源成果，均为 MIT 许可）
- **方法论框架（第一梯队）**：`obra/superpowers`（brainstorming / writing-plans / TDD / 根因调试 / 子智能体）、`addyosmani/agent-skills`（DEFINE→PLAN→BUILD→VERIFY→REVIEW→SHIP 六阶段、stop-the-line 六步调试、五轴评审、可观测）。
- **角色与技能库（第二梯队）**：`wshobson/agents`（ws-* 全栈角色、调试工具集、Python 开发技能，本地已大量同步）、`Jeffallan/claude-skills`（Feature Forge / Debugging Wizard 工作流）、`anthropics/skills`（mcp-builder、skill-creator、webapp-testing 方法论）。
- **Java / JVM 专项（用户指定，本技能 04 的主要事实来源）**：
  - `amplicode/spring-skills`：Spring 工具厂商官方，结构化工程上下文、CRUD/DTO/Mapper 生成、IntelliJ Debug MCP 安全规则；
  - `rrezartprebreza/spring-boot-skills`：Spring Boot 3/4 生产级规则（分层/六边形/DDD、REST 契约、事务、JPA、测试金字塔、安全、韧性、迁移、可观测），其 "Gotchas" 专治 AI 生成 Spring 代码的高频幻觉；
  - `wallaceespindola/ai-agents`：企业级 Java/Spring Cloud/Quarkus 技能（微服务、配置中心、响应式、GraalVM native、Maven/Gradle、JVM 性能、安全审计）。
- **用户锚点**：本地 `fullstack-flow`（五阶段流水线、闸门、状态落盘、专家路由、DoD），本技能延续其范式并扩展为多语言、多主线总控。

> 致谢说明：本文件是对上述成果的**提炼、翻译与编排整合**，不是逐字搬运；具体规则以各上游仓库最新版本为准，版本相关特性（如 Spring Boot 3 vs 4）以项目实际依赖为准。
