# tester

验证功能是否符合约定，并把浏览器用户流程变成可重复执行的自动化测试。

## 怎么工作

一般功能验证按 **明确范围 → 选择检查 → 实际执行 → 测试报告** 完成。需要浏览器自动化时，按下面的阶段推进：

```text
需求 / 验收 / 改动
       │
  1. Explore     探索页面，产出测试计划
       ├─ 1.5 Sync（可选）  把计划同步到来源工单
       │
  2. Generate    从计划生成 spec
       │
  3. Execute     独立运行，保存测试报告和证据
       └─ 失败 → Heal → 复验
```

| 现在手里有什么 | 从哪里开始 | 得到什么 |
| --- | --- | --- |
| 需求或改动，还没有计划 | [Explore](skills/sprite-tester-verification/references/e2e/explore.md) | 用户步骤、预期、覆盖缺口和可生成案例 |
| 已有明确计划 | [Generate](skills/sprite-tester-verification/references/e2e/generate.md) | 与计划对应的测试代码，随后执行 |
| 已有 spec | [Execute](skills/sprite-tester-verification/references/e2e/ci-execution.md) | 实际通过/失败/受阻结果与证据 |
| 失败或不稳定测试 | [Heal](skills/sprite-tester-verification/references/e2e/heal.md) | 修复与复验，或可复现的产品/环境问题 |

完整流程与完成要求见 [E2E 工作流](skills/sprite-tester-verification/references/e2e/workflow.md)。已有成果从相应阶段继续；只要计划或诊断时就在该范围结束。

## 直接这样使用

接入后，向 AI 描述任务即可：

> 请用 tester，按当前验收要求和实际改动做功能验证，记录结果、问题和未覆盖部分。

> 请用 tester 完成这次改动的 E2E：先探索并写计划，再生成测试、执行和修复失败。

> 请用 tester 修复这个失败用例，保留原验收预期，复验后更新报告。

本职能力入口是 `sprite-tester-verification`，包含一般验证和完整 E2E 流程。阶段文档随同一个 Skill 安装，按任务读取；不需要其他角色、完整 PRD、Jira 或固定浏览器 MCP。

## 产物与完成标准

- 计划、测试报告和必要证据放业务仓库 `docs/tester/<feature-id>/`，也可直接放 `docs/tester/`；spec、fixture、POM 和配置沿用工程原目录。
- 探索用于制定计划，执行结果由正式 runner 证明。新增/修复 E2E 默认需两次无重试隔离和一次无重试相关组通过，完整套件按项目 CI 要求执行。
- 未执行、受阻、失败和覆盖缺口如实保留。通过结论说明版本与适用范围；测试角色不替代产品决定或最终发布决定。

完整报告与交接要求见 [交付与留存](skills/sprite-tester-verification/references/delivery.md)，跨 Git 使用见 [产物交接说明](docs/artifact-handoff.md)。

## 接入当前项目

把下面这段粘贴给项目中的 AI：

```text
请阅读 https://github.com/ai-sprites/tester 的 README 和接入手册，把 tester 角色及 sprite-tester-verification 完整 Skill 接入当前项目，包括全部工作流、参考和模板。
固定同一明确版本，原样复制完整内容，只适配当前客户端的目录、元数据和入口引用；保留项目规则与自定义。资料来源和业务仓库已有选择就沿用，缺项合并问一次，允许稍后配置。
完成后核对文件与相对引用，说明版本、保存位置、调用方式和实际加载状态。接入和后续维护按手册完成，取不到源文件时说明缺项，不自行改写替代。
```

[接入手册](docs/installation.md) 包含完整复制、已有内容更新、资料选择和加载检查。客户端未自动加载时，让 AI 先读取已保存的角色文件或 Skill 入口；只有聊天权限则在会话中使用，并明确未写入项目。

## 资源清单

角色负责职责与检查清单；Skill 提供实际工作方法。默认同时接入角色和完整 Skill，可单独使用 Skill。所有下列资源来自同一版本，按需读取，不要求每次任务读完。

<details>
<summary>展开完整资源清单</summary>

| 资源 | 用途 |
| --- | --- |
| [角色定义](templates/agent.md) | 职责、边界和完成检查 |
| [Skill 入口](skills/sprite-tester-verification/SKILL.md) | 根据当前任务直接选择流程 |
| [一般验证](skills/sprite-tester-verification/references/workflow.md) | 范围、检查、执行和报告 |
| [交付与留存](skills/sprite-tester-verification/references/delivery.md) | 模板、目录、历史、索引和版本 |
| [测试报告模板](skills/sprite-tester-verification/assets/templates/test-report.md) | 正式验证报告 |
| [E2E 工作流](skills/sprite-tester-verification/references/e2e/workflow.md) | 阶段、产物和完成标准 |
| [Phase 1 · Explore](skills/sprite-tester-verification/references/e2e/explore.md) | 页面探索与计划 |
| [Phase 1.5 · Sync](skills/sprite-tester-verification/references/e2e/sync-test-cases.md) | 可选工单同步 |
| [Phase 2 · Generate](skills/sprite-tester-verification/references/e2e/generate.md) | 从计划生成 spec |
| [Phase 3 · Execute](skills/sprite-tester-verification/references/e2e/ci-execution.md) | 运行、CI 和执行证据 |
| [Heal](skills/sprite-tester-verification/references/e2e/heal.md) | 失败诊断与复验 |
| [E2E 计划模板](skills/sprite-tester-verification/assets/templates/e2e-plan.md) | 案例、步骤、预期和交接 |
| [项目适配](skills/sprite-tester-verification/references/e2e/project-conventions.md) | 实际目录、fixture 和命令 |
| [认证与环境](skills/sprite-tester-verification/references/e2e/auth-and-environment.md) | 登录、网络和测试数据 |
| [探索工具](skills/sprite-tester-verification/references/e2e/exploration-tooling.md) | 浏览器及 probe/debug 回退 |
| [选择器](skills/sprite-tester-verification/references/e2e/selectors-and-locators.md) | 定位与控件断言 |
| [可靠性](skills/sprite-tester-verification/references/e2e/reliability-and-readiness.md) | 就绪顺序和八类不稳定问题 |
| [请求模拟](skills/sprite-tester-verification/references/e2e/request-mocking.md) | 安全、窄范围的响应补丁 |

</details>

资源清单对应当前页面或 checkout 的版本；接入时固定完整 Git 提交，再取该提交的整套资源。历史 `v0.2.0` 仍可明确选用，内容以该标签为准。追加、更新或移除资源按 [手册](docs/installation.md#已有内容与后续维护) 处理，保留自定义和未选资源。
