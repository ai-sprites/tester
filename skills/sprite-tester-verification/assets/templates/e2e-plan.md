# E2E 测试计划：<功能>

<!-- 保存到 docs/tester/<feature-id>/e2e-plan.md；替换占位内容。每个用例保留表格行与详情。Remote sync 是可选区，无同步的新计划可删除该区；已有身份和历史必须保留。 -->

## What changed

<哪些行为发生变化、已确认预期、主要回归风险>

## Sources

- Requirement / acceptance: <真实验收依据，区分确认决定与建议>
- Ticket: <来源系统、真实 key/URL，或 none>
- Change: <真实 PR、提交或变更文件>
- Existing coverage: <相关测试文件及已覆盖行为，或 none>
- Tested version / environment: <应用和测试版本、环境>

## Project setup

- Commands: <已核实的列举、隔离、相关组、静态检查命令及 project/筛选范围>
- Test code / fixtures: <工程真实目录、fixture/辅助层和导入入口>
- Environment / data: <登录和数据准备、隔离与清理方法，不写凭据>
- Evidence: <必要观察证据的位置，或 none>

## Test cases

| ID | Test case title | File | Status | Lifecycle |
| --- | --- | --- | --- | --- |
| TC-1 | <可观察行为标题，与 test 标题一致> | <实际 spec 路径> | unverified | active |

探索状态 `Status`：verified / verified with mock / blocked / unverified。它不表示自动化测试已通过。
生命周期 `Lifecycle`：active / obsolete。TC-n 不重排、不复用；obsolete 保留历史。仅 active 且 verified 或 verified with mock 可生成测试。

## Test case details

### TC-1 — <与表格相同的标题>

**File:** <实际 spec 路径>

**Setup:** <账号权限、上下文、fixture、数据和 mock 前置>

**Steps:**

1. <用户动作>
   - expect: <一个具体可观察结果>
   - readiness: <响应、加载状态或可重试 UI 断言>
2. <下一动作；按实际步骤增删>
   - expect: <一个具体可观察结果；多个结果各占一行>
   - readiness: <此步骤的实际就绪信号>

**Notes:**

- Hooks: <实际 role/name 或配置过的 test ID>
- Endpoints: <URL、method、预期 status、必要请求身份；无则 N/A>
- Mock: <范围、理由、响应依据，以及能区别于未模拟状态的断言；无则 none>
- Test data: <可复现输入与独立准备/恢复方法>
- Observation: <实际观察及必要证据，明确真实/模拟；未观察则注明>
- Blocked / unverified: <未解决事项、受影响预期和下一步，或 none>
- Lifecycle change: <取消/替代的依据，或 none>

> UNVERIFIED: <有未确认信息时保留；已解决则删除此提示并写回正文>

## Not covered

| 行为或分支 | 原因及影响 | 已有覆盖或后续检查 |
| --- | --- | --- |
| <缺口，或 none> | <原因> | <真实测试路径或下一步> |

## Open questions

- <仍影响范围、预期或数据的问题及关联 TC，或 none>

## Execution handoff

- Ready to generate: <符合状态条件的 TC，或 none>
- Deferred cases: <blocked/unverified/obsolete 的 TC 与原因，或 none>
- Verification report: <真实报告路径与状态；尚未执行写未执行>

## Remote sync（可选）

- Sync target: <已明确的系统与对象类型；未开启为 none>
- Plan sync ID: <不可变身份；从未同步为 N/A>
- Remote test plan: <pending 或已核验 key/URL；从未同步为 N/A>
- Sync status: <待授权、已验证、暂停、受阻或冲突；明确部分失败>

<!-- 暂停/断连不清空已有身份。旧 Sources 中的同步字段与 Jira Test Cases Sub-task 可兼容读取，冲突不得覆盖。工单同步成功、探索状态和测试执行结果分别记录。 -->
