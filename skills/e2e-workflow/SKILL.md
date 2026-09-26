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
        └─ 需要回写 Jira → e2e-sync-test-cases → 关联已有工单
        │
Phase 2 │ GENERATE  skill: e2e-generate，以明确验收和计划为输入
        │           └─ 工程测试目录中的 *.spec.ts
        │
Phase 3 │ EXECUTE   独立 runner 执行
        │           ├─ 通过 → verification.md + evidence/
        │           └─ 失败 → e2e-heal → 回到 EXECUTE 复验
```

| 阶段 | 输入 | 做什么 | 完成条件 |
| --- | --- | --- | --- |
| [1 · e2e-explore](../e2e-explore/SKILL.md) | 需求、改动和已有覆盖 | 尝试用户流程，写清步骤与预期 | 计划可独立交接，每个案例都有准确探索状态 |
| [e2e-sync-test-cases](../e2e-sync-test-cases/SKILL.md)（按任务需要） | 计划/结果、已有 Jira 工单 | 在授权内关联测试记录并回读 | 真实更新链接或具体受阻原因 |
| [2 · e2e-generate](../e2e-generate/SKILL.md) | 可生成的计划案例 | 复用项目 fixture/POM，逐条实现步骤和断言 | spec 与计划对应，交给执行阶段 |
| [3 · Execute](references/ci-execution.md) | spec、真实环境及项目命令 | 隔离运行、相关组验证，保存报告 | 相关测试实际执行，必要稳定性/回归检查完成，缺口如实记录 |
| [e2e-heal](../e2e-heal/SKILL.md)（失败分支） | 失败命令、计划、report/trace | 定位测试、产品、环境或需求问题 | 测试修复后复验，或交付可复现问题及受阻范围 |

已有计划直接进入 Generate，已有失败直接进入 Heal，按任务补齐必要阶段。需求与验收通过已有 Jira、文档或代码 MCP 按需读取，本地及用户材料也可用。Jira 回写按任务需要进行，受阻不影响本地测试。单元、组件、API 等任务使用 tester 角色的一般测试说明。

## 执行规则

1. **探索不等于测试通过。** 页面观察用于选案例；通过结论来自正式 runner 执行。
2. **生成依据明确。** 复用现有验收、计划和项目测试约定；只有用户流程或预期不清时才补探索。交接保留足够信息，不强制另开会话或子代理。
3. **区分预期与观察。** 验收、步骤和测试数据明确即可生成，未探索的部分如实标注；缺少关键预期只暂停相关案例。保留已有用例 ID 与有效历史。
4. **每条预期有断言，每次异步交互有就绪信号。** 遵循 arrange → arm → act → await → assert，不以 sleep、networkidle、吞错或加 retries 修失败。
5. **按风险验证。** 先运行新增/修复的相关测试，再按项目策略以及 flaky、共享状态或回归风险追加隔离、重复或组合验证。无法执行的项记受阻，重试通过不能掩盖首次失败。
6. **CI 独立运行。** 只依赖仓库 runner、配置、fixture 和正式凭据来源，不依赖 AI 会话、MCP、手动登录窗口或探索脚本。
7. **Jira 按需关联。** 用现有 MCP 读取需求不意味着要写回；任务要求同步时沿用已有工单与测试记录，不强制建子任务。

## 产物

沿用项目已有位置；无约定时可采用以下目录，不要求迁移历史文档：

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
- [ ] 本次案例的验收、步骤与预期清楚，观察状态和受阻/未验证项如实记录。
- [ ] 任务需要的 Jira 关联有真实结果或明确缺口。
- [ ] 测试对应计划或验收，用例身份及未实现覆盖可追溯。
- [ ] 必要就绪信号和行为断言完整，相关可靠性风险及 mock 边界明确。
- [ ] 相关测试、按风险追加的验证及项目要求的 CI 状态真实可查，缺口与失败保留。
- [ ] 交付测试代码、必要记录/证据和已有计划（若有）；仅清理本次无用 scratch。
