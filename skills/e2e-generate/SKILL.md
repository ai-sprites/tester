---
name: e2e-generate
description: "根据明确验收或测试计划生成 Playwright spec，复用项目 fixture 和页面对象，并衔接实际执行。"
---

# Phase 2 — GENERATE：从计划生成测试

**输入：**已明确的验收或测试计划，以及目标项目的测试配置和辅助代码。

**输出：**工程原生目录中的 spec 及必要辅助代码，可追溯到验收或已有计划案例。此阶段完成代码准备，真实运行在 [Phase 3 — EXECUTE](../e2e-workflow/references/ci-execution.md) 完成。

## 1. 确认生成范围

从明确验收或现有计划核对目标行为、步骤、前置条件、测试数据和可观察预期。信息充分即可生成，不强制先探索、另开上下文或重建计划；缺少关键预期或交互依据时，按需回 Explore 补充，只暂停受影响案例。观察、推断和实际执行结果分别记录。

读取[项目适配](../e2e-workflow/references/repo-conventions.md)、邻近 spec、fixture 和交互辅助代码，确认真实导入与文件位置。

## 2. 将计划映射为代码

沿用项目测试组织和命名方式，每个 test 可追溯到对应验收或计划案例。需要拆分、合并或改名时，同步受影响的计划引用，不重复维护同一份需求。

有计划时关联真实路径与用例 ID；没有计划时引用验收来源。每条必要预期落实为可观察断言，复杂步骤可使用项目现有 step 机制。

下面仅演示标准 Playwright 的对应关系。实际使用时替换计划/spec 路径、标题、页面地址、文案和定位方式；项目已有自定义 `test` fixture 或 POM 时，改用其真实导出和交互方法。

```ts
// plan: docs/tester/profile/e2e-plan.md
// covers: TC-1
import { test, expect } from "@playwright/test";

// TC-1 — 标题与计划一致
test("should require a display name", async ({ page }) => {
  // 1. 打开编辑页；expect: 名称输入框可编辑
  await page.goto("/profile/edit");
  const name = page.getByRole("textbox", { name: "Display name", exact: true });
  await expect(name).toBeEditable();

  // 2. 清空名称后保存；expect: 显示必填提示
  await name.fill("");
  await page.getByRole("button", { name: "Save", exact: true }).click();
  await expect(page.getByRole("alert")).toHaveText("Name is required");
});
```

## 3. 接入项目约定与就绪信号

复用项目的 fixture、POM/交互辅助层和常量。有 POM 时交互放 POM、业务断言留 spec；没有时按实际复用需要提取辅助方法。

编写前读[可靠性与就绪信号](../e2e-workflow/references/reliability-and-readiness.md)。按 arrange → arm → act → await → assert 安排异步步骤，选择真实需要的响应、加载状态或可重试 UI 断言。上例假设必填校验在客户端完成，以错误提示作为就绪结果；依赖服务端响应的步骤应在触发前建立监听，见可靠性参考中的示例。

定位遵循[选择器](../e2e-workflow/references/selectors-and-locators.md)。权限、上下文、数据和 [mock](../e2e-workflow/references/request-mocking.md) 在相关请求触发前准备好；mock 仅覆盖本次声明的边界，限定单测试和具体请求、保留响应契约，并用断言区分模拟状态。

每条断言验证验收或计划中的具体结果，不能用宽泛 truthy、页面非空、吞错、固定等待或额外重试掩盖失败。

## 4. 核对代码，交给执行阶段

检查验收/计划与测试的对应关系、导入、步骤、断言和必要辅助代码；按实际风险检查时序、隔离与 mock。未实现的必要覆盖说明原因。

应用与计划冲突时保留依据，修正错误测试或报告产品问题。改变预期必须有已确认验收或新决定支持。计划修改时保留 TC 和有效历史；本次需要回写 Jira 时，按[工单关联](../e2e-sync-test-cases/SKILL.md)更新已有记录。

## 完成条件与下一阶段

- 每个生成的 test 可追溯到验收或计划案例，步骤和断言完整。
- 文件、导入、fixture、定位和数据准备符合目标项目。
- 已核对时序、隔离、mock 范围及未生成案例。

默认继续 [Phase 3 — EXECUTE](../e2e-workflow/references/ci-execution.md)，执行测试发现、正式验证和报告留存；运行命令与通过要求统一以该阶段为准。用户只要求代码/草稿或明确不执行时，在此交付并标明尚未执行，不宣称测试通过。
