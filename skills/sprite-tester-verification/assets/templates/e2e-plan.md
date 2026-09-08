# E2E 测试计划

<!-- 填写前读完整模板；保存到业务仓库 docs/tester/<feature-id>/e2e-plan.md。替换所有占位内容，移除模板说明；代码放工程原生测试目录。已有计划可沿用等价结构，但适用字段不能缺失。 -->

## What changed

<本次变化、涉及的用户流程、已确认预期和主要回归风险>

## Sources

- Requirement / acceptance: <真实需求或验收来源；区分确认依据与建议>
- Ticket: <真实来源系统、工单 key/URL，或 none>
- Sync target: <none，或已明确的来源系统与对象类型，例如 Jira Sub-task>
- Plan sync ID: <开启同步时的不可变身份，新计划从未同步时为 N/A；已有身份即使暂停也保留>
- Remote test plan: <pending 或已回读验证的 key/URL，新计划从未同步时为 N/A；已有映射即使暂停也保留>
- Change: <真实 PR、提交或变更文件；没有则注明>
- Existing coverage: <已覆盖相关行为的真实测试文件或 none>
- Tested version / environment: <应用与测试版本、目标环境，不记录秘密>

<!-- 无真实来源工单时不联系工单系统；有来源但不需要同步时仍可关闭。已有有效映射不能因字段缺失、断连或暂停而清空；旧 Jira Test Cases Sub-task 字段应兼容读取，迁移保持 ID/key/标记，冲突写明受阻。 -->

## Project setup

- Test runner / command: <从包脚本或 CI 核实的列举、隔离、相关测试组命令及 project/过滤范围>
- Test code / fixtures: <实际目录、现有 fixture 或辅助层及入口>
- Environment / authentication: <准备方法和凭据来源名称；不填写凭据或 session 内容>
- Evidence: <本计划和验证证据的实际位置及访问限制>

## Test cases

<!-- 一行一个 test；标题与代码一致，语言和命名沿用项目。TC-n 永不重排复用，新 ID 高于历史最大值。Status 仅表示探索状态，不表示自动化已通过。 -->

| ID | Test case title | File | Status | Lifecycle |
| --- | --- | --- | --- | --- |
| TC-1 | <可观察行为标题> | <项目实际 spec 路径> | unverified | active |

`Status`：`verified`（真实应用已探索）、`verified with mock`（通过指定模拟探索）、`blocked`（有具体阻碍）、`unverified`（未尝试或预期未确认）。

`Lifecycle`：`active` 或 `obsolete`。obsolete 行保留身份、历史详情和失效依据；blocked/unverified 保留缺口，不生成假定已确认的测试。

## Test case details

### TC-1 — <与表格相同的标题>

**File:** <项目实际 spec 路径>

**Setup:** <所需权限、上下文、数据状态、fixture、mock；无额外前置则注明>

**Steps:**

1. <用户动作>
   - expect: <一个具体可观察结果>
   - readiness: <此步骤所需响应、加载状态或可重试 UI 断言>
2. <下一用户动作；无则删除该示例步骤>
   - expect: <一个具体可观察结果>
   - expect: <另一个结果；无则删除>
   - readiness: <实际需要的就绪信号>

**Notes:**

- Hooks: <已观察的 role/name 或配置过的 test ID；无则说明>
- Endpoints: <相关 URL/匹配条件、method、预期 status 及必要请求身份；不涉及则 N/A>
- Mock: <模拟范围、理由、真实响应依据及区别于未模拟状态的断言；无则 none>
- Test data: <可复现数据及隔离/清理方法；不包含秘密>
- Observation: <已观察结果及必要证据，区分真实与模拟；未观察则明确写出>
- Blocked / unverified: <具体原因、受影响预期与下一步，或 none>
- Lifecycle change: <失效/替代原因和依据，或 none>

> UNVERIFIED: <仅在有未确认信息时保留，说明哪项预期或前置条件待确认；已解决则移除并写回正文>

## Not covered

| 行为或分支 | 原因与影响 | 已有覆盖或后续检查 |
| --- | --- | --- |
| <范围缺口，或 none> | <未覆盖原因，不用未执行冒充通过> | <真实文件或具体后续检查> |

## Open questions

- <仍影响范围/预期/数据的未解决问题及所影响 TC，或 none；已得到的答案写回适用位置>

## Execution handoff

- Ready to generate: <active 且 verified/verified with mock 的 TC 列表，或 none>
- Deferred cases: <blocked/unverified/obsolete 的 TC 与原因，或 none>
- Verification report: <真实报告路径和当前状态；尚未执行明确写未执行>
- Remote sync: <关闭、待授权、已验证同步、受阻或冲突；如有部分失败如实说明>

<!-- 每个适用 expect 都应进入测试断言。计划和生成的测试一并交付；探索状态、远端同步成功与测试执行结果分别报告。 -->
