# 05 · Python 工程手册（要点 + 本地技能路由）

> 本地技能库的 Python 覆盖很强，本文件给最小可用要点与**路由表**：遇到对应主题先读本地技能 SKILL.md，按其方法执行，不重复造轮子。架构与流程仍走总控主线。

## 1. 开工前探测
找 `pyproject.toml/requirements.txt/poetry.lock/uv.lock/setup.py`，判明：Python 版本、依赖与环境管理（venv/poetry/uv/conda）、Web 框架（FastAPI/Flask/Django）、异步还是同步、测试（pytest/unittest）、类型检查（mypy/pyright）、格式化（ruff/black）。brownfield 沿用既有工具链，不擅自换。

## 2. 最小工程要点
### 项目结构与风格
- `src/布局`打包、清晰包边界；配置走环境变量/`pydantic-settings`，不硬编码；依赖固定锁版本。
- 类型标注全量，公共函数标注入参与返回；用 dataclass/pydantic model 表达结构化数据；异常分层定义，不裸 `except Exception` 吞掉。
- 路由本地：`python-project-structure`、`python-code-style`、`python-configuration`、`python-type-safety`、`python-packaging`。

### FastAPI / Web
- 路由层薄、业务在 service；Pydantic 模型分离请求/响应（对应 Java 的 DTO 分离）；依赖注入用 `Depends`；统一异常处理器 `@app.exception_handler`；后台/重活用任务队列不在请求里阻塞。
- 路由本地：`fastapi-templates`。

### async 并发（高频翻车点）
- 不要在 async 函数里做同步阻塞调用（`requests`、`time.sleep`、CPU 密集、同步 DB 驱动）——会堵死事件循环；I/O 用 `httpx.AsyncClient`/异步驱动，CPU 密集用 `run_in_executor`/进程池。
- 每个外部 I/O 必设超时；用 `asyncio.gather` 并发并妥善处理部分失败；理解协程必须被 await、任务要持有引用防被回收。
- 路由本地：`async-python-patterns`、`python-background-jobs`、`python-resilience`（重试/退避/熔断）。

### 错误处理与可观测
- 只捕获能处理的异常，捕获后要么恢复、要么带上下文 re-raise（`raise X from e`）；用结构化日志（`logging`/structlog），不用 print 上生产；关键路径加 trace/指标。
- 路由本地：`python-error-handling`、`error-handling-patterns`、`python-observability`、`python-resource-management`（上下文管理器）。

### 性能
- 先 profiling（cProfile/py-spy/line_profiler）再优化；善用生成器处理大数据流、内置数据结构与算法复杂度；缓存只加在证明的热点；DB 侧同样先排 N+1（异步 ORM 也一样）。
- 路由本地：`python-performance-optimization`。

### 测试
- pytest：fixture 管理依赖、`pytest.mark.parametrize` 覆盖正常/边界/错误、mock 外部 I/O 但不 mock 掉被测行为；集成测试用 testcontainers 起真实依赖；覆盖率 80%+，断言质量优先。
- 路由本地：`python-testing-patterns`；通用 TDD 见 `08`。

### 常见反模式（评审对照）
可变默认参数 `def f(x=[])`（默认值在定义时创建、跨调用共享）；闭包晚绑定（循环里建 lambda 要绑默认参数）；浮点当等值判断；字典链式 `[]` 不判 KeyError；全局可变状态；同步 I/O 阻塞事件循环；不设超时的网络调用；循环里逐条查库。

## 3. Python 后端 → 本地技能路由总表
| 主题 | 本地技能 |
|---|---|
| 整体 Web 模板 | `fastapi-templates` |
| 异步/并发 | `async-python-patterns` |
| 后台任务/队列 | `python-background-jobs` |
| 配置管理 | `python-configuration` |
| 设计模式 | `python-design-patterns` |
| 错误处理 | `python-error-handling`、`error-handling-patterns` |
| 可观测/日志 | `python-observability` |
| 弹性（重试/熔断） | `python-resilience` |
| 资源管理 | `python-resource-management` |
| 性能 | `python-performance-optimization` |
| 项目结构/打包 | `python-project-structure`、`python-packaging` |
| 代码风格/类型 | `python-code-style`、`python-type-safety`、`python-anti-patterns` |
| 测试 | `python-testing-patterns` |
| LLM/Agent | `langchain-architecture`、`rag-implementation`、`llm-evaluation`（配合 07） |

## 4. 交付前 Python 自检
- [ ] `ruff`/lint、类型检查、`pytest` 真实通过；依赖锁文件已更新
- [ ] 无 print 调试残留、无硬编码密钥、无裸 except、无可变默认参数
- [ ] async 路径无阻塞调用，外部 I/O 都有超时
- [ ] 正常/边界/错误三类测试齐全，bug 有回归测试
