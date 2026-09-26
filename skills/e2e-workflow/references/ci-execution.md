# Phase 3 — Execute

**输入：** 待验证的 spec、可用环境、项目测试命令，以及关联计划（若有）。

**输出：** 项目现有测试报告与证据；无约定时可用 `docs/tester/<feature-id>/verification.md`。

## 1. 确认将运行什么

有计划时，从 `Project setup` 复用已核实的命令；直接运行已有 spec 时，从项目配置读取，不为运行测试强建计划。首次执行或配置有变化时，按 [项目适配](repo-conventions.md) 核对 runner、版本、project、筛选范围与 setup/teardown；确认目标案例被发现，零测试或跳过不算执行。

检查 [认证与环境](auth-and-environment.md) 中本次依赖的条件。浏览器能打开页面不等于测试 runner 已就绪；环境受阻时记录具体命令和现象，继续可独立的检查。

## 2. 运行相关测试，按风险追加验证

先运行新增/修复用例及直接受影响的测试。按项目既有要求执行必要的 lint、类型检查等；相关检查已通过且没有新变化或未解决风险时，不无理由重复或扩大全量运行。

根据证据追加检查：

- flaky、时序或偶发失败：关闭 retries 隔离执行，按复现条件重复验证，保留每次结果。
- 共享数据、fixture 或顺序依赖：运行相关测试组，核对状态泄漏与清理。
- 跨模块或发布回归风险：使用项目规定的更大范围或 CI 矩阵。

以下只是已安装 Playwright 且项目允许直接调用时的示例；路径、筛选和次数按实际任务调整，不擅自下载或升级 runner：

```sh
npx playwright test path/to/spec.spec.ts --list
npx playwright test path/to/spec.spec.ts --retries=0
# 仅在稳定性问题需要重复验证时
npx playwright test path/to/spec.spec.ts:12 --repeat-each=2 --retries=0
```

使用 `:12` 等过滤时先核对目标 test 的真实位置和发现结果；零测试或 skip 不算验证。项目入口不能关闭 retries 时保留首次失败与重试记录，不用最终绿灯冒充稳定通过。

失败进入 [Heal](../../e2e-heal/SKILL.md)，修复后复验受影响检查。说明无法执行的范围与限制。

## 3. 核对 CI 结果

完整套件按项目已有 CI/验收要求运行，本地不默认跑全量。CI 必须通过独立 runner 和项目配置准备环境、认证与数据，不依赖 AI 会话、MCP、手动登录窗口或探索 scratch。

从对应 run 取得 commit、环境、命令、报告、首次失败、retry 和最终结果。浏览器矩阵、分片、workers、触发规则及 artifact 名都读取真实 CI 配置；本地与 CI 不一致时逐项比较这些条件，再检查依赖和数据。

已有 CI 结果要核对是否覆盖本次版本。CI 未运行、未配置或报告不可获取就明确记录；迁入此工作流不会自动创建流水线、定时任务、通知或凭据。

报告和 trace 使用当前已安装 runner 的查看器打开，路径从实际 CI artifact 中取得，例如：

```sh
npx playwright show-report path/to/report
npx playwright show-trace path/to/trace.zip
```

## 4. 保存结论与证据

正式验证参考 [测试报告模板](test-report.md) 更新报告；单次简单检查可直接回复。记录每项命令、版本、环境、通过/失败/未运行/受阻及证据。总体判定仅覆盖实际检查范围，说明未覆盖部分和剩余风险。

工程 runner 与 CI 产物保持原位置。查看实际 report/trace 后，在报告中链接支撑结论的证据；确需保存副本时先脱敏，无法访问或保存时说明限制。按 [交付与留存](delivery.md) 更新已有报告和引用，只清理本次无用 scratch。

## 完成检查

- [ ] 目标案例实际执行，项目要求及风险所需验证已完成，未满足项明确记录。
- [ ] 项目要求的静态检查与 CI 状态准确，未执行不写通过。
- [ ] 交付包含失败、缺口、剩余风险和有适用范围的判定。
- [ ] 交付 spec、报告（若有）、必要证据和关联计划（若有）的真实路径；没有计划时直接说明测试身份及验收依据。
