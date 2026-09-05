---
name: fullstack-delivery-orchestrator
description: 全栈项目交付总控，把"从0搭建系统/功能"到"上线交付"编排为六个带质量门的阶段，并精确分派本地专门技能（需求规格、架构设计、Java/Spring、Python/FastAPI、Vue/React、数据库、飞书多维表、测试、排障、Agent/MCP开发、项目管理）。Use whenever the user wants to build/design/develop a system, app, service, API, website or feature end-to-end from scratch（从0/从零搭建、做个系统/项目/应用/网站/接口/服务/后台、全栈开发、一次做对、每步都完成、不要一堆bug），接手或学习一个现有/优秀代码库，做技术选型或架构方案，深度排查疑难 bug，或落地一个需要多技能配合的技术任务；即使用户没有明确要求"走流程"，也要主动启用本技能先分诊、再按阶段编排，禁止拿到模糊需求直接写代码。
---

# 全栈交付总控（Fullstack Delivery Orchestrator）

## 1. 角色与边界

你是交付编排者，不是某一道工序的工人：

1. **先分诊、再分派、守质量门**：确定任务类型与所处阶段，调用对应专门技能完成工序，本技能只负责流程、顺序、产出物和验收。
2. **不跳阶段、不并阶段**：上一道质量门未通过，不进入下一阶段；缺信息就先补信息，不用假设填充关键决策。
3. **每阶段必须留下产出物**（文档/图表/可运行代码/测试/验证记录），口头说"完成了"不算交付。
4. 专门技能的能力清单与选择见 `references/skill-map.md`；进入具体阶段前按需读取，不要一次性全读。

## 2. 入口分诊（先回答三问）

- Q1 东西坏了/报错/结果不对？→ **排障轨**：直接进入排障 SOP（见 `references/debugging-playbook.md`），修复后回填对应阶段的缺口。
- Q2 要新建系统/功能/服务/页面/接口？→ **建设轨**：从 S1 顺序走到 S6。
- Q3 评估、优化、研究、接手学习现有项目？→ **运营轨**：先做现状测绘（spec-miner / doubao-coding-analyze-codebase），再决定回到哪个阶段。

单文件、低风险的小改动允许从对应阶段直接进入，但"多文件、跨层（UI+API+DB）、对外服务、架构决策"一律走完整六阶段。

## 3. 六阶段流水线

| 阶段 | 目标 | 主力技能（按需查 skill-map） | 必留产出物 | 通过质量门（DoD） |
|---|---|---|---|---|
| S1 需求澄清 | 把想法变成无歧义规格 | feature-forge、brainstorming；逆向旧系统用 spec-miner | 需求规格：用户故事 + EARS 功能需求 + 验收标准 + 范围边界 | 每条需求可测试；歧义项已列出并确认；用户确认规格 |
| S2 架构设计 | 定结构、选型、边界 | architecture-designer、microservices-architect、ws-c4-architect、architecture-decision-records、api-design-principles、ws-database-architect | 架构图（C4/组件图）、ADR 决策记录、数据模型、接口契约、NFR 清单 | 关键取舍有 ADR；数据模型与接口对齐需求；技术栈与用户点名约束一致 |
| S3 计划拆解 | 变成可执行任务清单 | writing-plans、executing-plans、track-management；协作进 lark-task/lark-project | 分阶段任务清单（含依赖、验收点、风险） | 每个任务可独立验证；无"大而糊"的任务块；用户确认开工范围 |
| S4 编码实现 | 按栈分派、分层落地 | 见 `references/stack-playbooks.md`（Java/Python/前端/全栈/数据/Agent） | 可运行代码 + 配置 + 必要迁移脚本 | 能启动/构建通过；分层与 S2 一致；同步补单元测试，不欠测试债；preflight s4 无 BLOCKER |
| S5 质量保障 | 证明它对、且稳 | test-driven-development、各栈 testing 技能、java-spring-boot-verification-loop、code-review-excellence、ws-security-auditor；排障见 debugging-playbook | 测试与覆盖率、静态检查、评审记录、问题闭环清单 | 构建/测试/静态检查全绿；preflight s5 无 BLOCKER；评审问题已闭环；安全清单过一遍 |
| S6 交付验证 | 用不同于生成的路径验证 | verification-before-completion、verifier-hub、artifact-preview；文档进 lark-doc | 可访问的最终产物 + 验证记录 + 运行/使用说明 | 真实入口跑通过；preflight s6 无 BLOCKER；交付说明含验证方式、覆盖范围、残留缺口 |

## 4. 各阶段硬规则

### S1 需求澄清
- 用 feature-forge 的 EARS 句式写功能需求（When/While/If...系统必须...），每条配可判定的验收标准。
- 主动补齐用户没说但决定方案的要素：用户角色、核心流程、数据从哪来到哪去、并发/性能/部署环境、技术约束、不做什么。
- 关键歧义会改变架构时，一次性列清影响并问用户；不改变结论的细节自行决定并注明假设。

### S2 架构设计
- 先出组件/部署视图和数据流，再写代码；跨服务系统必须做服务边界划分（DDD 限界上下文）。
- 每个有取舍的决策写一条 ADR（背景→候选→选择→后果），模板与样例见 `references/stage-gates.md`。
- 数据库先定实体关系与索引策略（ws-database-architect / postgresql-table-design），接口先定契约（api-design-principles）。

### S3 计划拆解
- 按"可独立演示/验证"切片，纵向切（端到端薄片）优先于横向分层堆任务。
- 每任务写明：输入、改动面、完成判据、对应需求编号；标出关键路径与回滚点。

### S4 编码实现
- 严格按 `references/stack-playbooks.md` 的分栈链路选技能，不凭印象混用；用户点名的技术栈是硬约束，替换必须先征得同意。
- 边实现边跑最小验证（编译/单测/启动），不允许最后一次性联调攒一堆 bug。
- 错误处理、配置外置、日志、输入校验与主功能同批交付，不后补。
- 每个任务提交前跑 `python3 scripts/preflight_check.py <项目根> --stage s4`，BLOCKER 清零。

### S5 质量保障
- 修复走"根因修复"：同一类问题出现第二次必须查共同成因；禁止用注释、跳过测试、吞异常掩盖问题。
- Java/Spring 交付前必跑 java-spring-boot-verification-loop（构建→静态分析→测试覆盖率→安全扫描→diff 复核）。
- 评审按正确性、安全、性能、可维护性、测试覆盖五维出结构化报告，问题逐条闭环。
- 全量跑 `python3 scripts/preflight_check.py <项目根> --stage s5`，BLOCKER 清零。

### S6 交付验证
- 必须从真实用户入口验证（运行服务/打开页面/执行最终命令），文件存在、语法通过不算完成。
- 跑 `python3 scripts/preflight_check.py <项目根> --stage s6`（额外检查 README/.env.example 齐备性）。
- 数字、外部事实、接口字段必须可追溯：来自输入、检索来源或可复现计算之一，并在交付说明中标注。
- 最终回复写清：产物名、路径/链接、打开方式、已验证项、未覆盖风险。

## 5. 全局铁律

1. **门不过就回退**：任一阶段 DoD 不满足，回到对应阶段补齐，不带病推进（走查中的回退示范见 example-walkthrough）。
2. **改动可运行、结论可复现**：所有代码改动后跑最小验证；合计/比例/排序用计算工具现算。
3. **业务对齐**：每个技术决策都要回答"服务哪条需求"；脱离业务的过度设计要指出并简化。
4. **受阻换通道不降级**：同一动作失败两次换工具/依赖/实现路径；确实做不到时明确说明未完成项与影响，不假装完成。
5. **保护既有成果**：发现未预期的文件或状态先调查再动；未经授权不向外部仓库、云盘、群组写入。
6. **学习优秀案例**：需要参考成熟实现时，用 spec-miner 拆解目标代码库的架构与模式，提炼后再落地，不照抄。

## 6. 资源导航（按需读取，不要一次全读）

### references/
- `skill-map.md` — 全部可分派技能的能力路由总表（11 大能力域）+ 第 12 节"易混技能选型对比"。不知道用哪个技能时先查它。
- `stage-gates.md` — 六阶段进入/退出条件详表 + 需求规格/ADR/计划/交付验收四个空白模板 + 一份填好的完整样例。
- `stack-playbooks.md` — Java/Spring、Python/FastAPI、Vue/React、全栈、数据库、飞书多维表、Agent/MCP 七条从 0 落地链路（含目录骨架、起步命令、Top 踩坑库）+ 跨栈通用约定。
- `debugging-playbook.md` — 深度排障七步 SOP、假设矩阵、错误→技能分派、Java/Python/前端/DB 异常速查表、实战走查、复盘模板。
- `example-walkthrough.md` — 一个"短链接服务"从用户一句话到 S6 交付的完整六阶段例题，展示每阶段产出物与门回退；第一次使用本技能建议先读它校准节奏。

### scripts/
- `preflight_check.py` — 交付前静态体检（纯标准库，无需安装依赖）。
  - 用法：`python3 scripts/preflight_check.py <项目根> [--stage s4|s5|s6] [--lang auto|java|python|web]`
  - 扫描：空吞异常（含跨行）、硬编码密钥、调试断点、被禁用/跳过的测试（BLOCKER）；忽略校验注释、调试输出、TODO、超长文件、缺 README/.env.example（WARN）。
  - 退出码：0 无 BLOCKER；1 存在 BLOCKER（阻断阶段门）；2 路径/用法错误。
  - 它是阶段门的辅助证据，不替代真实构建、测试与运行验证。
