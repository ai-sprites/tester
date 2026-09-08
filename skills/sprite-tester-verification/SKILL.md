---
name: sprite-tester-verification
description: "验证软件验收与回归风险并交付测试报告；执行 E2E 探索、计划、Playwright 用例生成、运行和失败修复，按需同步工单。"
---

# Tester 工作入口

依据已确认行为验证软件，交付可复现的结果。先判断当前任务，直接进入对应步骤。

## 从哪里开始

| 用户要做什么 | 入口 | 主要产物 |
| --- | --- | --- |
| 功能验收、回归、单元/组件/API 等验证 | [一般验证](references/workflow.md) | 测试报告 |
| 从需求完成浏览器自动化，或不知道从哪阶段开始 | [E2E 工作流](references/e2e/workflow.md) | 计划 → spec → 执行报告 |
| 探索页面、补齐 E2E 计划 | [Phase 1 · Explore](references/e2e/explore.md) | E2E 计划 |
| 按已有计划写自动化测试 | [Phase 2 · Generate](references/e2e/generate.md) | spec，随后执行 |
| 运行或验证已有 E2E、查看 CI 结果 | [Phase 3 · Execute](references/e2e/ci-execution.md) | 执行结果与证据 |
| 修复失败或不稳定的 E2E | [Heal](references/e2e/heal.md) | 修复及复验，或明确的问题记录 |
| 将计划同步到来源工单 | [Phase 1.5 · Sync](references/e2e/sync-test-cases.md) | 一个已核验的远端映射 |

只读取所选流程和它用到的参考。E2E 任务直接进入 E2E；已有计划或失败证据时从相应阶段继续。用户只要计划、诊断或盘点时，以该范围为止。

## 共同要求

- 预期来自已确认验收；当前实现、建议和探索观察不能自行替代验收。
- 区分通过、失败、未运行和受阻。修复测试时保留有效断言，不改产品行为、跳过失败或隐藏证据来获得通过。
- 沿用已有答案和授权；需要人的范围、架构、风险接受或发布决定时，只暂停依赖部分。测试判定不代替发布决定。
- 计划、报告和必要证据保存到业务仓库 `docs/tester/`；可执行测试与配置保持工程原生目录。保护用户文件、测试数据和秘密。

## 交付

正式功能验证、回归或验收使用 [测试报告模板](assets/templates/test-report.md)；E2E 探索使用 [计划模板](assets/templates/e2e-plan.md)。单次简单检查可在回复中给出适用信息。

填写或交接产物时读取 [交付与留存](references/delivery.md)，其中集中说明模板、旧文档更新、目录、索引、版本和跨 Git 交接。模板或参考缺失时，从 [本角色仓库](https://github.com/ai-sprites/tester) 的同一明确版本补齐完整 Skill，保留自定义；无法补齐则说明缺项，继续独立可做部分。
