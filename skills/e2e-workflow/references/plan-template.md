# Plan template

复制到 `docs/tester/<feature-id>/e2e-plan.md` 并填写。计划应让另一会话只凭计划和项目代码就能写出测试，不依赖探索聊天或临时笔记。用例标题使用简单英文，并以 `should` 开头。

---

# <Feature> test plan

## What changed

<用 3–5 句话说明哪个页面或流程发生变化、用户现在能做什么或看到什么，以及主要回归风险。>

## Sources

- Requirement / acceptance: <真实验收依据，区分已确认决定与建议>
- Ticket: <真实 Jira key 与链接，或 none>
- Plan sync ID: <不可变 ticket-key/feature-slug；无工单且从未同步时为 N/A>
- Jira Test Cases Sub-task: <pending、已核验的 Sub-task key，或无工单且从未同步时为 N/A>
- Change: <真实 PR、提交或变更文件>
- Existing E2E: <已有相关覆盖的 spec 路径，或 none>
- Tested version / environment: <应用和测试版本、实际环境>

真实 Jira 工单进入 [e2e-sync-test-cases](../../e2e-sync-test-cases/SKILL.md) 的准备和预览；首次建立稳定身份后，Sub-task 映射在创建并核验前使用 `pending`。无工单的新计划两个映射都写 `N/A`，不发现或调用 Jira 工具。已有身份、key 和显式暂停不得因断连、字段缺失或迁移被清空；必要时在此注明同步的暂停、受阻或部分失败。

## Project setup

- Commands: <已核实的测试列举、隔离、相关组与必要静态检查命令，包含 project/筛选范围>
- Test code / fixtures: <工程真实目录、fixture/POM/辅助层及导入入口>
- Environment / data: <认证与数据准备、隔离和恢复方法，不写凭据>
- Evidence: <必要观察证据的位置，或 none>

## Test cases

每个案例一行，对应一个 `test()`；简单英文标题以 `should` 开头，生成时逐字使用。TC-n 不重排、不复用，新 ID 大于历史最大值。

| ID | Test case title | File | Status | Lifecycle |
| --- | --- | --- | --- | --- |
| TC-1 | should show an error when a required field is empty | `<实际 spec 路径>` | unverified | active |
| TC-2 | should keep the saved value after reloading | `<实际 spec 路径>` | unverified | active |

`Status` 表示探索状态：

- `verified`：已在真实应用驱动流程并观察结果。
- `verified with mock`：已驱动流程，使用范围明确的响应补丁；记录模拟边界。
- `blocked`：环境、权限或数据阻止探索；写明原因与下一步。
- `unverified`：尚未尝试，或范围、预期仍未确认。

这些状态不代表自动化测试通过。`Lifecycle` 为 `active` 或 `obsolete`；保留 obsolete 行、详情和历史 ID，不为其生成测试。仅 active 且 verified / verified with mock 的案例可以生成；未解决的案例保留为缺口，不能默默删除。

## Test case details

每一行重复一个详情块，标题与表格完全一致。

### TC-1 — should show an error when a required field is empty

**File:** `<实际 spec 路径>`

**Setup:** <账号权限、上下文、fixture、独立数据和 mock 前置>

**Steps:**

1. <用户动作>
   - expect: <一个具体可观察结果>
   - readiness: <该步骤需要的响应、加载状态或可重试 UI 断言>
2. <下一用户动作；按实际步骤增删>
   - expect: <一个具体可观察结果>
   - expect: <另一个具体可观察结果；每个结果独立一行>
   - readiness: <此步骤的真实就绪信号>

**Notes:**

- Hooks: <实际 role/name、label 或已配置的 test ID>
- Endpoints: <URL、method、预期 status 与必要请求身份；无则 N/A>
- Why mocked: <范围、理由、已确认响应依据及能区分未模拟状态的断言；无则 none>
- Test data: <可复现输入及独立准备/恢复方法>
- Observation: <实际观察和必要证据，区分真实/模拟；尚未观察须注明>
- Blocked / unverified: <未解决事项、受影响预期及下一步，或 none>
- Lifecycle change: <取消或替代的确认依据，或 none>

> UNVERIFIED: <只保留尚未确认的信息；解决后将答案写回正文并删除对应提示>

## Not covered

列出本次改动涉及但未由本计划测试的行为，并用一句话说明原因。已有覆盖应列真实测试路径。

| What | Why it is not an E2E test | Existing coverage / next check |
| --- | --- | --- |
| <行为或分支，或 none> | <原因及影响> | <真实路径或后续检查> |

## Open questions

- <仍影响范围、预期或数据的问题及关联 TC；没有则写 none>

---

## Rules for this plan

- 每个案例包含一个表格行和一个详情块，使用相同的稳定 TC-n、简单英文 `should ...` 标题、文件、setup 和编号用户步骤。一个计划分组对应一个 describe，一个案例对应一个 test，测试标题逐字沿用计划。
- 写用户动作，不用 API 调用或代码代替步骤。每个产生可见结果的步骤至少有一条 `expect:`；每条 `expect:` 对应至少一个具体断言。
- 每个异步依赖步骤写出真实就绪信号，按 arrange → arm → act → await → assert 编排。纯 UI 操作不捏造网络等待，不用固定 sleep 补缺口。
- 覆盖正常流程、重要错误或边界，以及本次变化涉及的权限、上下文、功能开关、实体状态、后端错误和持久化分支；已有有效覆盖可以引用。
- 各案例能独立准备和恢复数据，不依赖执行顺序。无法安全构造的服务端状态只修改必要响应字段，保留真实响应契约，并说明模拟未证明的集成行为。
- 未确认信息标为 `UNVERIFIED`，保留 blocked / unverified 缺口；已有答案写回正文，不能按当前实现反写验收或无依据删除范围。
- 保留 `Plan sync ID`、TC-n、obsolete 历史和已核验的 Sub-task key。每份有真实来源工单的计划只同步一个普通 Jira Sub-task，标题以 `[Test Cases][Auto-Created]` 开头，描述包含完整计划和身份标记；按已获授权写入，缺授权时先预览再询问。
- 无工单且从未同步的计划使用 `N/A` 映射并跳过所有 Jira 操作。此前 `Remote sync` / `Remote test plan` / `Sync target` 字段按同步 Skill 的兼容规则读取；保留已有身份与显式暂停，不另建重复对象。
- 计划与产生的测试一起交接或提交。探索状态、工单同步和正式执行结果分别记录；实际执行与缺口保存到 `docs/tester/<feature-id>/verification.md`，必要证据放同目录 `evidence/`。

本计划由 [e2e-explore](../../e2e-explore/SKILL.md) 编写，由 [e2e-sync-test-cases](../../e2e-sync-test-cases/SKILL.md) 同步，由 [e2e-generate](../../e2e-generate/SKILL.md) 生成测试。详细规则见[选择器](selectors-and-locators.md)、[请求模拟](request-mocking.md)与[可靠性和就绪信号](reliability-and-readiness.md)。
