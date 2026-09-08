# Phase 2 — GENERATE：从计划生成测试

**输入：**`docs/tester/<feature-id>/e2e-plan.md` 或已明确的等价计划，以及目标项目的测试配置和辅助代码。

**输出：**工程原生目录中的 spec 及必要辅助代码，与计划 TC 对应。此阶段完成代码准备，真实运行在 [Phase 3 — EXECUTE](ci-execution.md) 完成。

## 1. 确认生成范围

从计划开始，不读取探索笔记来补隐含步骤。核对 TC、表格与详情、标题、目标文件、setup、步骤及 `expect:`；只生成 `Lifecycle: active` 且 `Status: verified` / `verified with mock` 的案例。已有等价计划可以使用不同列名，依据真实内容对应状态与字段；有充分观察证据时补齐缺失状态，没有证据则回 Explore 补验证，不能默认视为 verified。重复 ID、未确认预期和阻碍只暂停受影响案例。

读取[项目适配](project-conventions.md)、邻近 spec、fixture 和交互辅助代码，确认真实导入与文件位置。

## 2. 将计划映射为代码

默认一个案例对应一个 test、一个分组对应 describe，沿用项目已有等价组织方式。标题与计划一致，每个 test 可辨识对应 TC；拆分、合并或改名时同步映射。

spec 写明从 Git 根可解析的计划路径与覆盖 ID。每个用户步骤保留对应注释或项目 step 机制，每条 `expect:` 落到具体可观察断言。

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

编写前读[可靠性与就绪信号](reliability-and-readiness.md)。按 arrange → arm → act → await → assert 安排异步步骤，选择真实需要的响应、加载状态或可重试 UI 断言。上例假设必填校验在客户端完成，以错误提示作为就绪结果；依赖服务端响应的步骤应在触发前建立监听，见可靠性参考中的示例。

定位遵循[选择器](selectors-and-locators.md)。权限、上下文、数据和 [mock](request-mocking.md) 在相关请求触发前准备好；mock 仅覆盖计划声明的边界，限定单测试和具体请求、保留响应契约，并用断言区分模拟状态。

每条断言验证计划中的具体结果，不能用宽泛 truthy、页面非空、吞错、固定等待或额外重试掩盖失败。

## 4. 核对代码，交给执行阶段

检查计划与测试映射、导入、步骤、断言和必要辅助代码；对照可靠性参考的八类常见问题检查时序与隔离。生成范围应与可执行案例一致，未生成的 TC 留明原因。

应用与计划冲突时保留依据，修正错误测试或报告产品问题。改变预期必须有已确认验收或新决定支持。计划修改时保留 TC 和同步身份；已开启同步的计划重新计算[远端差异](sync-test-cases.md)。

## 完成条件与下一阶段

- 每个生成的 test 可追溯到计划 TC，步骤和断言完整。
- 文件、导入、fixture、定位和数据准备符合目标项目。
- 已核对时序、隔离、mock 范围及未生成案例。

默认继续 [Phase 3 — EXECUTE](ci-execution.md)，执行测试发现、正式验证和报告留存；运行命令与通过要求统一以该阶段为准。用户只要求代码/草稿或明确不执行时，在此交付并标明尚未执行，不宣称测试通过。
