# E2E 工作流

把已确认需求变成能独立运行的浏览器测试。默认采用 Playwright；已采用其他框架的项目沿用原执行方式。框架、目录和命令在首次接触或相关配置变化时核对，记录后复用。

## 工作顺序

```text
需求 / 验收 / 实际改动
        │
Phase 1 │ EXPLORE   在真实页面探索
        │           └─ docs/tester/<feature-id>/e2e-plan.md
        │
        ├─ 需要工单同步 → Phase 1.5 SYNC → 一个测试计划子任务
        │
Phase 2 │ GENERATE  只以计划为输入生成测试
        │           └─ 工程测试目录中的 *.spec.ts
        │
Phase 3 │ EXECUTE   独立 runner 执行
        │           ├─ 通过 → verification.md + evidence/
        │           └─ 失败 → HEAL → 回到 EXECUTE 复验
```

| 阶段 | 输入 | 做什么 | 完成条件 |
| --- | --- | --- | --- |
| [1 · Explore](explore.md) | 需求、改动和已有覆盖 | 尝试用户流程，写清步骤与预期 | 计划可独立交接，每个案例都有准确探索状态 |
| [1.5 · Sync](sync-test-cases.md)（可选） | 计划、真实来源工单及同步请求 | 对账、预览差异、按授权写入并回读 | 一份计划对应一个已验证远端对象，或明确同步受阻 |
| [2 · Generate](generate.md) | 可生成的计划案例 | 复用项目 fixture/POM，逐条实现步骤和断言 | spec 与计划对应，交给执行阶段 |
| [3 · Execute](ci-execution.md) | spec、真实环境及项目命令 | 隔离运行、相关组验证，保存报告 | 两次隔离和一次相关组均无重试通过；其余结果如实记录 |
| [Heal](heal.md)（失败分支） | 失败命令、计划、report/trace | 定位测试、产品、环境或需求问题 | 测试修复后复验，或交付可复现问题及受阻范围 |

完整自动化任务顺序走完；已有计划直接进入 Generate，已有失败直接进入 Heal。同步受阻不阻止本地测试。单元、组件、API 等任务走 [一般验证](../workflow.md)。

## 六条执行规则

1. **探索不等于测试通过。** 页面观察用于选案例；通过结论来自正式 runner 执行。
2. **生成从计划开始。** 另一会话仅凭计划和项目代码就应能写测试，不依赖探索聊天或 scratch。
3. **只生成已验证的 active 案例。** `blocked`、`unverified` 保留缺口；`obsolete` 保留历史；`TC-n` 永不重排或复用。
4. **每条预期有断言，每次异步交互有就绪信号。** 遵循 arrange → arm → act → await → assert，不以 sleep、networkidle、吞错或加 retries 修失败。
5. **执行条件明确。** 新增/修复 E2E 按 Phase 3 完成两次无重试隔离和一次无重试相关组验证；无法执行的项记受阻。完整套件按项目 CI/验收要求运行。
6. **CI 独立运行。** 只依赖仓库 runner、配置、fixture 和正式凭据来源，不依赖 AI 会话、MCP、手动登录窗口或探索脚本。

## 产物

```text
docs/tester/<feature-id>/
├── e2e-plan.md       用例、步骤、预期和探索状态
├── verification.md   实际执行、失败、缺口和判定
└── evidence/         必要日志、截图、trace 等证据
```

spec、POM、fixture 和可执行配置放工程原目录。只清理本次无用 scratch，保留支持结论的证据。填写和交接时见 [交付与留存](../delivery.md)。

## 需要时再读

| 遇到的问题 | 参考 |
| --- | --- |
| 不清楚当前项目的目录、fixture 或运行命令 | [项目适配](project-conventions.md) |
| 登录、会话、网络或测试数据不可用 | [认证与环境](auth-and-environment.md) |
| 浏览器工具无法访问，或需要 probe/debug 回退 | [探索工具](exploration-tooling.md) |
| 选择器、受控输入或自定义控件断言 | [选择器](selectors-and-locators.md) |
| 竞态、动画、重渲染、debounce 或 mock 时序 | [可靠性与就绪信号](reliability-and-readiness.md) |
| 需要安全构造服务端分支 | [请求模拟](request-mocking.md) |
