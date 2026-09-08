---
name: e2e-workflow
description: "浏览器 E2E 自动化总入口：选择探索、工单同步、测试生成、独立执行或失败修复阶段，并读取共享约定。"
---

# E2E 工作流

这五个 Skill 把已确认需求变成能独立运行的 Playwright 浏览器测试。框架、目录和命令在首次接触或相关配置变化时核对，记录后复用。

## 工作顺序

```text
需求 / 验收 / 实际改动
        │
Phase 1 │ EXPLORE   skill: e2e-explore，在真实页面探索
        │           └─ docs/tester/<feature-id>/e2e-plan.md
        │
        ├─ 有来源 Jira 工单 → e2e-sync-test-cases → 一个普通 Sub-task
        └─ 无来源工单 → 跳过 Jira
        │
Phase 2 │ GENERATE  skill: e2e-generate，独立上下文以计划为输入
        │           └─ 工程测试目录中的 *.spec.ts
        │
Phase 3 │ EXECUTE   独立 runner 执行
        │           ├─ 通过 → verification.md + evidence/
        │           └─ 失败 → e2e-heal → 回到 EXECUTE 复验
```

| 阶段 | 输入 | 做什么 | 完成条件 |
| --- | --- | --- | --- |
| [1 · e2e-explore](../e2e-explore/SKILL.md) | 需求、改动和已有覆盖 | 尝试用户流程，写清步骤与预期 | 计划可独立交接，每个案例都有准确探索状态 |
| [1.5 · e2e-sync-test-cases](../e2e-sync-test-cases/SKILL.md)（有 Jira 工单时） | 计划、真实来源 Jira 工单 | 对账、预览差异、按授权写入并回读 | 一份计划对应一个已验证远端对象，或明确同步受阻 |
| [2 · e2e-generate](../e2e-generate/SKILL.md) | 可生成的计划案例 | 复用项目 fixture/POM，逐条实现步骤和断言 | spec 与计划对应，交给执行阶段 |
| [3 · Execute](references/ci-execution.md) | spec、真实环境及项目命令 | 隔离运行、相关组验证，保存报告 | 两次隔离和一次相关组均无重试通过；其余结果如实记录 |
| [e2e-heal](../e2e-heal/SKILL.md)（失败分支） | 失败命令、计划、report/trace | 定位测试、产品、环境或需求问题 | 测试修复后复验，或交付可复现问题及受阻范围 |

完整自动化任务顺序走完；已有计划直接进入 Generate，已有失败直接进入 Heal。同步受阻不阻止本地测试。单元、组件、API 等任务使用 tester 角色的一般测试说明。

## 执行规则

1. **探索不等于测试通过。** 页面观察用于选案例；通过结论来自正式 runner 执行。
2. **生成从独立计划开始。** 新会话或隔离子代理只接收计划和所需项目代码，不传探索笔记；同一会话继续时也只按计划重新推导步骤，不依赖隐含观察。
3. **只生成已验证的 active 案例。** `blocked`、`unverified` 保留缺口；`obsolete` 保留历史；`TC-n` 永不重排或复用。
4. **每条预期有断言，每次异步交互有就绪信号。** 遵循 arrange → arm → act → await → assert，不以 sleep、networkidle、吞错或加 retries 修失败。
5. **执行条件明确。** 新增/修复 E2E 按 Phase 3 完成两次无重试隔离和一次无重试相关组验证；无法执行的项记受阻。完整套件按项目 CI/验收要求运行。
6. **CI 独立运行。** 只依赖仓库 runner、配置、fixture 和正式凭据来源，不依赖 AI 会话、MCP、手动登录窗口或探索脚本。
7. **有来源 Jira 工单就交接同步，无工单跳过 Jira。** 一份计划对应一个普通 Sub-task，标题以 `[Test Cases][Auto-Created]` 开头。先预览，按已有授权写入；缺授权只在具体差异准备好后询问。显式暂停和已有映射按同步 Skill 保留处理。

## 产物

```text
docs/tester/<feature-id>/
├── e2e-plan.md       用例、步骤、预期和探索状态
├── verification.md   实际执行、失败、缺口和判定
└── evidence/         必要日志、截图、trace 等证据
```

spec、POM、fixture 和可执行配置放工程原目录。只清理本次无用 scratch，保留支持结论的证据。填写和交接时见 [交付与留存](references/delivery.md)。

## 需要时再读

| 遇到的问题 | 参考 |
| --- | --- |
| 不清楚当前项目的目录、fixture 或运行命令 | [项目适配](references/repo-conventions.md) |
| 登录、会话、网络或测试数据不可用 | [认证与环境](references/auth-and-environment.md) |
| 浏览器工具无法访问，或需要 probe/debug 回退 | [探索工具](references/exploration-tooling.md) |
| 选择器、受控输入或自定义控件断言 | [选择器](references/selectors-and-locators.md) |
| 竞态、动画、重渲染、debounce 或 mock 时序 | [可靠性与就绪信号](references/reliability-and-readiness.md) |
| 需要安全构造服务端分支 | [请求模拟](references/request-mocking.md) |
| 编写可独立交接的计划 | [计划模板](references/plan-template.md) |
| 执行本地检查与 CI | [CI 执行](references/ci-execution.md) |

## 全流程完成检查

- [ ] 已读需求、来源工单（若有）、改动与现有覆盖。
- [ ] 候选案例已探索或明确标注受阻/未验证，计划包含稳定 TC、用户步骤及逐条预期。
- [ ] 有 Jira 工单的计划已交接 `e2e-sync-test-cases`；无工单未调用 Jira。
- [ ] 每个 active 且已验证案例对应一个 test，一个计划组对应一个 describe，标题逐字沿用计划。
- [ ] 逐步就绪信号和断言完整，检查过八类可靠性问题及 mock 边界。
- [ ] 本地两次隔离、一次相关组及要求的 CI 状态真实可查，缺口与失败保留。
- [ ] 计划、测试代码、报告与必要证据一起交付；仅清理本次无用 scratch。
