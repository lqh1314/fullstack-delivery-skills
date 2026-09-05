---
name: fullstack-flow
description: 全栈项目端到端交付流水线，单入口串联 需求业务澄清 → 数据库与架构设计 → 分层实现 → 测试/安全/性能三路评审 → 部署与文档交接，含审批闸门、状态落盘、失败即停与根因调试。当用户要做全栈应用、端到端新功能、前后端加数据库联动开发、从零搭项目，或说“用全栈流程/走完整开发流程/full stack/端到端做一个”时必须使用本 Skill；即使未点名，只要任务同时涉及前端(UI)、后端(API/服务)、数据库中两层及以上，也应主动使用。单文件小修、纯 bug 修复、纯问答、只改样式不适用，改用对应单点 Skill。
---

# FullStack Flow — 全栈交付流水线

## 这个 Skill 是什么

把“一个功能从模糊想法到可上线交付”的完整过程固化成**一条单入口流水线**，整合三套已验证方法：

- **九阶段全栈编排**（源自本地 `ws-full-stack-feature` / wshobson 方法）：阶段顺序、产出落盘、审批闸门；
- **superpowers 工程纪律**（源自本地 `brainstorming` / `writing-plans` / `test-driven-development` / `verification-before-completion` / `systematic-debugging`）：需求硬门禁、TDD、完成前真实验证、根因调试；
- **gstack sprint 方法**：业务前提拷问（office-hours）、QA 真实运行找 bug、每个修复附带回归测试。

本 Skill 是**编排器**：负责流程顺序、产出物、闸门和质量门；具体专业判断切换到对应“专家视角”（见 `references/role-cards.md`），运行时若已安装对应本地专家 Skill（ws-* 等），直接读取并复用其方法，不重复造轮子。

## 五条第一原则（流程为什么长这样）

1. **落盘，不凭记忆。** 每个阶段把产出写进 `.fullstack-flow/`，下一阶段**读文件**而不是依赖上下文记忆。长任务会压缩上下文，落盘是唯一可靠的交接方式。
2. **闸门处必须停。** 需求、架构、测试结果三处需要用户明确批准才能继续——跑偏的成本随阶段指数上升，越早拦住越便宜。
3. **失败即停，先找根因。** 任何阶段报错、测试失败、依赖缺失，立即停机并按 `systematic-debugging` 方法定位根因，禁止带着失败继续往下堆代码。
4. **质量内建，三路并行。** 测试、安全、性能在实现完成后同时评审；Critical/High 问题修复并复验后才允许交付。
5. **仪式随复杂度伸缩。** 小功能可以合并阶段、缩短文档，但三个审批闸门和“完成前真实验证”不可省略；拿不准时就走更重的流程（复杂度只会在中途暴露，不会消失）。

## Phase 0：启动与会话恢复

1. 检查项目根目录是否存在 `.fullstack-flow/state.json`：
   - 存在且 `status=in_progress`：读出 feature 与 current_step，询问用户「续跑 / 重开（归档旧会话）」；
   - 存在且 `status=complete`：询问是否归档后开始新功能；
   - 不存在：新建。
2. 探测项目类型与技术栈（不要直接问用户能自己查到的东西）：查找 `package.json`、`requirements.txt/pyproject.toml`、`go.mod`、`Cargo.toml`、`pom.xml/build.gradle`、框架配置文件、迁移目录，判断 greenfield / brownfield 与前后端框架、数据库、测试框架。
3. 初始化状态文件：

```json
{
  "feature": "功能描述",
  "status": "in_progress",
  "project_type": "greenfield|brownfield",
  "stack": {"frontend": "", "backend": "", "database": "", "test": "", "infra": ""},
  "complexity": "simple|medium|complex",
  "current_phase": 1,
  "completed_phases": [],
  "files_created": [],
  "started_at": "ISO时间",
  "last_updated": "ISO时间"
}
```

解析用户输入中的标志：`--no-frontend`（纯后端/API）、`--no-db`（无持久化）、`--simple`（合并阶段、轻文档）、`--complex`（完整仪式）。未指定时按探测结果与功能影响面判定，并在需求阶段向用户确认。

## 流程总览

| 阶段 | 做什么 | 必产文件（.fullstack-flow/） | 闸门 | 详细指引 |
|---|---|---|---|---|
| 1 需求与业务 | 业务拷问 + 验收标准 + 范围边界 | `01-requirements.md` | **闸门 A：需求确认** | `references/01-requirements.md` |
| 2 设计 | 数据模型 + 前后端架构 + 风险 | `02-database-design.md`、`03-architecture.md` | **闸门 B：架构评审** | `references/02-design.md` |
| 3 实现 | DB 层 → 后端 → 前端，严格按此顺序 | `04-db-impl.md`、`05-backend-impl.md`、`06-frontend-impl.md` | — | `references/03-implementation.md` |
| 4 验证 | 测试套件 + 安全审计 + 性能审计，三路并行 | `07-testing.md` | **闸门 C：测试评审** | `references/04-testing.md` |
| 5 交付 | CI/CD 与部署 runbook + 文档与交接 | `08-deployment.md`、`09-documentation.md` | 完成定义核验 | `references/05-delivery.md` |

每完成一个阶段：更新 `state.json`（current_phase、completed_phases、files_created、last_updated），再进入下一阶段。

## 各阶段执行要点（细节读对应 reference）

### Phase 1 需求与业务 —— 闸门 A

- **一次只问一个问题**，等用户回答再问下一个；给出 2-3 个建议选项降低用户负担。
- 先做业务拷问（这个功能解决谁的什么痛点、没有它用户现在怎么办、成功是什么样），再问验收标准、明确不做什么、技术约束、依赖。完整问题单与模板见 `references/01-requirements.md`。
- 没有验收标准的需求不允许进入设计。产出 `01-requirements.md` 后，**停下来**用简明摘要请用户确认 / 修改 / 暂停。

### Phase 2 设计 —— 闸门 B

- 先数据后架构：先出 ER 关系、表/集合 schema、索引与迁移策略、查询模式、数据访问接口；再出后端（接口、服务层边界、鉴权、集成点）与前端（组件层级、状态管理、路由、取数与缓存策略），以及错误传递链、安全与风险清单。
- brownfield 项目必须先读现有模型、接口约定与目录结构，新设计沿用既有模式。
- 产出 `02`、`03` 后停下来，展示关键表、接口清单、组件结构摘要，请用户批准后才允许写实现代码。

### Phase 3 实现

严格按 **DB 层 → 后端服务 → 前端界面** 的顺序，后一步读前一步的落盘摘要：

1. 迁移脚本 / 模型 / 数据访问层；
2. 接口与业务逻辑、输入校验、错误码、鉴权、结构化日志；
3. 组件、状态流、接口对接、表单校验与错误/加载态、响应式与可访问性基础。
- 遵循项目既有代码风格与约定；每步结束写实现摘要（创建/修改了哪些文件）。
- `--no-frontend` / `--no-db` 时跳过对应步骤，但要在对应文件里写明“为何跳过”，保持流水线完整。

### Phase 4 验证 —— 闸门 C（三路并行）

实现完成后**同时**从三个视角审查，缺一不可：

- **A 测试套件**：新业务逻辑单测、接口集成测试、数据层/迁移测试、前端组件测试（如适用）；覆盖正常路径、边界、错误路径；新代码覆盖率目标 80%+；优先 TDD 红-绿-重构。
- **B 安全审计**：OWASP Top 10、鉴权与越权、输入校验、注入、XSS/CSRF、敏感数据、依赖漏洞，按严重度给出位置与修法。
- **C 性能审计**：N+1、缺失索引、未优化查询、缓存机会、大 payload、前端不必要重渲染与包体。

Critical / High 发现必须**当场修复并复验**；每个 bug 修复要附带回归测试（防止复发）。汇总为 `07-testing.md`，展示覆盖率与各级问题数，停下来等用户批准。详细清单见 `references/04-testing.md`。

### Phase 5 交付

- 部署：CI/CD 流水线、迁移步骤、特性开关（如需灰度）、健康检查与就绪探针、核心指标告警、**含数据库回滚的部署 runbook**；
- 文档：接口请求/响应示例、schema 变更与迁移说明、关键决策 ADR、交接摘要（做了什么、怎么测、已知限制）；
- 最后按「完成定义」逐项真实验证（见下），通过后把 state 置为 `complete` 并输出总结。

## 复杂度裁剪规则

| 情况 | 裁剪方式 |
|---|---|
| Spike（可行性试探，“能不能做”） | 不走全流程：说明试探计划→用户同意→最小代价验证→给结论，产物标注一次性 |
| Bounded（现有代码中的小改动，单接口/单组件） | Phase 1/2 合并为聊天内短设计，用户点头后实现；Phase 4 只做相关测试 + 安全自查；仍需闸门与真实验证 |
| Architectural（新项目/新子系统/接口重构） | 完整五阶段，不裁剪 |
| 纯后端 API（`--no-frontend`） | 跳过前端实现与组件测试，其余完整 |
| 中途发现变复杂 | 立即停机说明，升级到更重路径，不允许悄悄降级 |

## 专家视角路由

每个阶段切换到对应专家思维模式，关注点、产出物与典型陷阱见 `references/role-cards.md`：
业务分析 → 数据架构师 → 全栈架构师 → 后端工程师 → 前端工程师 → QA/TDD 编排 → 安全审计 → 性能工程 → 发布工程师。
本地已装对应专家 Skill（如 `ws-database-architect`、`ws-backend-architect`、`ws-frontend-developer`、`ws-tdd-orchestrator`、`ws-security-auditor`、`ws-performance-engineer`、`ws-deployment-engineer`）时，读取其 SKILL.md 后按其方法执行。

## 出问题时的固定动作

任何阶段连续失败、测试红、构建失败、结果异常：停止推进，切换到根因调试——复现 → 最小化定位 → 提出假设并验证 → 做原因级修复 → 加回归测试；同一修复尝试失败 3 次必须停下重新假设，不允许盲目试错。本地有 `systematic-debugging` / `doubao-coding-diagnose-and-fix-bugs` 时直接复用。

## 完成定义（Definition of Done，逐项打勾）

- [ ] 三个闸门都有用户明确批准记录；
- [ ] 代码在真实入口可运行（不是“看起来对”）：构建/类型检查/lint 通过；
- [ ] 测试套件实际运行通过，覆盖率与 `07-testing.md` 记录一致；
- [ ] Critical/High 安全与性能问题清零并复验；
- [ ] 迁移可正向执行、回滚路径在 runbook 中可操作；
- [ ] `09-documentation.md` 交接信息齐全，state.json 置 complete；
- [ ] 向用户汇报：产物清单、验证方式与结果、已知限制、下一步（PR / 部署）。
