# 从计划生成测试

输入是 `docs/tester/<feature-id>/e2e-plan.md` 或已明确的等价计划，不是探索会话。读取[项目适配](project-conventions.md)、相关 fixture/辅助代码和[可靠性](reliability-and-readiness.md)后再生成。

## 计划与代码对应

1. 核对稳定 ID、表格与详情、标题、目标文件、setup、步骤及 `expect:`。只生成 `Lifecycle: active` 且 `Status: verified` 或 `verified with mock` 的案例；缺字段、重复 ID、阻碍与未确认预期先澄清受影响部分。
2. 默认一个计划案例对应一个 test、一个计划分组对应 describe；沿用项目已有等价组织方式。测试标题与计划一致。合并、拆分或重命名时同步映射，不能丢失案例身份或预期。
3. spec 注明从 Git 根可解析的计划路径及覆盖 ID，例如 `// plan: docs/tester/search/e2e-plan.md`、`// covers: TC-1, TC-2`。沿用已有更合适的注解格式也可，但必须能追溯；每个 test 自身可辨识对应 TC。
4. 每个用户步骤旁保留与计划对应的步骤注释或项目 step 机制，每条 `expect:` 对应具体可观察断言。不要用宽泛 truthy、页面非空或“无异常”代替要求的用户结果。

## 实现方式

- 使用项目真实的 `test`/`expect` 导出及 fixture。已有自定义 fixture 时不绕开它；没有时可使用测试框架的标准入口，不凭空发明 fixture、常量文件或 import 路径。
- 复用 POM 或现有交互辅助层；有 POM 时交互放 POM、业务断言留 spec。项目没有该层时，仅在复用收益明确时提取辅助代码，不为一次测试强建框架。
- 定位遵循[选择器](selectors-and-locators.md)，沿用实际 test ID 配置。权限、上下文、数据和 mock 在触发相关请求前准备好。
- 依赖异步数据的步骤按 arrange → arm → act → await → assert 执行；为每个步骤选择实际需要的就绪信号，不给纯本地交互硬加网络等待。
- [mock](request-mocking.md)仅模拟计划声明的边界，限定单测试和具体请求，保留真实响应契约，断言应能辨别目标模拟状态。

## 验证与交付

使用从目标项目核实的 runner、project、过滤范围及格式检查命令，不直接执行未替换的示例命令。

1. 确认新增/更新的案例确实被发现，测试数量与计划生成范围对应。
2. 关闭重试，运行变更用例或 spec 两次隔离检查；一次 `--repeat-each=2 --retries=0` 或两次等价命令均可，确保独立数据与会话。
3. 关闭重试运行一次相关测试组，检查共享状态和交互回归；测试组来自真实目录、tag 或配置。保留项目要求的 lint、类型或格式检查；完整套件由项目 CI/验收规则决定。
4. 记录精确命令、被测版本、通过/失败/受阻及实际证据到[测试报告](../../assets/templates/test-report.md)。通过仅限真正执行的范围，探索 verified 不算执行通过。

用[可靠性参考](reliability-and-readiness.md)检查常见时序问题。失败转[诊断与修复](heal.md)，不通过增加 retries 或修改产品行为把结果变绿。应用与计划冲突时保留依据，修正错误测试或报告产品问题；只有已有验收或新确认决定支持时才改变预期。

计划内容改变时保留 TC 与同步 ID，已开启同步的计划重新计算[远端差异](sync-test-cases.md)。交付计划、spec、报告与必要证据的真实路径，保留旧产物有效内容。
