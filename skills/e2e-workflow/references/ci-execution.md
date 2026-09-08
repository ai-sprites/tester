# Phase 3 — Execute

**输入：** 待验证的 spec、可用环境、项目测试命令，以及关联计划（若有）。

**输出：** `docs/tester/<feature-id>/verification.md` 与必要证据。

## 1. 确认将运行什么

有计划时，从 `Project setup` 复用已核实的命令；直接运行已有 spec 时，从项目配置读取，不为运行测试强建计划。首次执行或配置有变化时，按 [项目适配](repo-conventions.md) 核对 runner、版本、project、筛选范围与 setup/teardown；确认目标案例被发现，零测试或跳过不算执行。

检查 [认证与环境](auth-and-environment.md) 中本次依赖的条件。浏览器能打开页面不等于测试 runner 已就绪；环境受阻时记录具体命令和现象，继续可独立的检查。

## 2. 运行变更用例与相关测试组

对新增或修复的 E2E，依次完成：

1. **每个目标用例单独运行两次，关闭 retries。** 筛选后只包含该用例及必要 setup/teardown，两次都必须真正执行并成功；同一 spec 中有多个案例时，不能用整份文件重复运行冒充单用例隔离。
2. **相关测试组运行一次，关闭 retries。** 按共享 fixture、功能、目录或 tag 选择范围，检查状态泄漏和回归。
3. **运行项目已有的必要静态检查。** 例如本次适用的格式、lint、类型检查。

下面演示已安装 Playwright、项目使用 npm 且支持直接调用 runner 时的参数组合。执行前把路径和入口换成计划中核实的值；自定义脚本还需核对参数转发，不擅自下载或升级 runner。

```sh
npx playwright test path/to/spec.spec.ts:12 --list
npx playwright test path/to/spec.spec.ts:12 --repeat-each=2 --retries=0
npx playwright test path/to/related-group --retries=0
```

示例的 `:12` 要替换为目标 test 的真实定义行号，并先用 `--list` 确认仅选中一个目标用例；参数化用例需继续用 runner 支持的过滤方式收窄。

项目有更严格要求时沿用。若现有入口无法关闭 retries，保留真实结果，但明确无重试稳定性检查尚未满足。重试后通过单列为 flaky，不用最终绿灯遮住首次失败。

失败时进入 [Heal](../../e2e-heal/SKILL.md)，修复后回到本步骤复验。检查已通过且没有新改动、失败或未解决风险时，不再无理由重跑。

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

使用 [测试报告模板](test-report.md)，记录每项命令、版本、环境、通过/失败/未运行/受阻及证据。总体判定仅覆盖实际检查范围，说明未覆盖部分和剩余风险。

工程 runner 的临时输出保持原配置。查看实际 report/trace 后，把支撑结论的必要附件脱敏留存到 `docs/tester/<feature-id>/evidence/`；不能下载或安全导出时列真实来源和访问限制。按 [交付与留存](delivery.md) 更新已有报告、引用和索引，只清理本次无用 scratch。

## 完成检查

- [ ] 目标案例确实被执行，两次隔离和一次相关组均无重试通过，或未满足项已明确记录。
- [ ] 项目要求的静态检查与 CI 状态准确，未执行不写通过。
- [ ] 报告包含失败、缺口、剩余风险和有适用范围的判定。
- [ ] 交付 spec、报告、必要证据和关联计划（若有）的真实路径；没有计划时，在报告记录测试身份及验收依据。
