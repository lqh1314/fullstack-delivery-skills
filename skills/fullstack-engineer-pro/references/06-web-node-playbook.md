# 06 · Web 前端 / Node / TypeScript 手册（要点 + 本地技能路由）

> 本地技能库的前端/Node 覆盖很强（ws-frontend-developer、react-*、nodejs-*、modern-javascript-* 等）。本文件给最小要点与路由表，专业细节读本地技能。

## 1. 开工前探测
读 `package.json`：包管理器（npm/pnpm/yarn）、框架（React/Vue/Next/Vite/Express/Nest）、语言（JS/TS）、构建工具、测试（Vitest/Jest/Playwright）、状态管理、样式方案。锁文件与 Node 版本（`.nvmrc/engines`）为准，不擅自升级大版本或换框架。

## 2. 前端最小要点
### 组件与状态
- 分层：页面/容器组件负责取数与状态，展示组件纯渲染；组件单一职责，别让一个组件同时承担取数+业务+渲染。
- 区分**服务端状态**（用 React Query/SWR 等管理，带缓存/失效/重取）与**本地 UI 状态**；避免到处复制同一份状态。
- 任何异步渲染处理好 **loading / empty / error 三态**；请求竞态要处理（忽略过期响应/AbortController）；表单防重复提交、做提交态与后端错误回显。
- 性能：避免不必要重渲染（合理 memo/key 稳定/状态下沉）、路由与大组件懒加载、大依赖不进主包、长列表虚拟化；先 profiling（React DevTools/Lighthouse）再优化。
- 可访问性：语义化标签、键盘可达、焦点管理、对比度、表单 label。
### 质量
- TS 严格模式，类型不用 `any` 糊弄；统一错误边界防白屏；用户输入渲染防 XSS（不 `dangerouslySetInnerHTML` 注入未消毒内容）。
- 组件测试用 Testing Library（测行为不测实现细节），关键流程 E2E 用 Playwright；样式与响应式遵循项目既有设计系统。
- 路由本地：`ws-frontend-developer`、`frontend-design`、`react-modernization`、`react-state-management`、`react-native-design`（移动端）、`responsive-design`、`mobile-android/ios-design`、`modern-javascript-patterns`、`web-component-design`、`interaction-design`、`accessibility-compliance`、`e2e-testing-patterns`、`javascript-testing-patterns`。

## 3. Node / 后端最小要点
- 分层同总控（路由薄、service 承载业务、数据访问隔离）；用 TS 类型贯穿；统一错误处理中间件，不把未处理异常泄漏给客户端。
- 所有外部调用（DB/HTTP/消息）设超时与重试边界；请求体大小、限流、输入校验（zod/joi 等）在边界完成；敏感配置走环境变量。
- 异步错误要被捕获（async 路由错误传给错误中间件 / 全局 `unhandledRejection` 处理）；流与连接要正确关闭防泄漏；集群/进程管理与优雅关停（SIGTERM 后停止接收新请求、处理完存量再退出）。
- 路由本地：`nodejs-backend-patterns`、`fastapi` 不适用、`api-design-principles`、`openapi-spec-generation`、`graphql` 相关见 `ws-graphql-architect`、`auth-implementation-patterns`、`session-guard`、`secrets-management`、`microservices-patterns`、`observability` 相关、`nodejs-backend-patterns`。

## 4. 前端/Node 常见反模式（评审对照）
只做 happy path 不处理错误/空态；错误只 `console.error` 用户无感知；`useEffect` 依赖缺失或无限渲染；列表 key 用数组下标；在渲染中直接改 state；fetch 无取消/无竞态处理；前端做鉴权判断当唯一防线；把密钥/令牌打进前端包；同步重计算阻塞主线程；无分页一次拉全量；npm 包版本不锁。

## 5. 交付前 Web 自检
- [ ] `lint`、`typecheck`、`build`、测试真实通过；构建产物体积无异常膨胀
- [ ] 主路径在真实浏览器跑通，三态齐全、无控制台报错/告警
- [ ] 无密钥进前端包、无 XSS 隐患、键盘可达
- [ ] 接口契约与后端一致（请求/响应/错误码），异常态有 UI
