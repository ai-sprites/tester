# tester

测试角色与五个独立的 E2E Skill。每个 Skill 有自己的 `SKILL.md`，名称、阶段分工和共享参考目录一一保留。

## 五个 Skill

| Skill | 负责什么 | 产出 |
| --- | --- | --- |
| [e2e-workflow](skills/e2e-workflow/SKILL.md) | 总流程、阶段选择、执行标准 | 明确下一阶段和完成条件 |
| [e2e-explore](skills/e2e-explore/SKILL.md) | 读需求、检查已有覆盖、探索页面 | 可独立交接的测试计划 |
| [e2e-sync-test-cases](skills/e2e-sync-test-cases/SKILL.md) | 有来源 Jira 工单时，同步一份完整计划 | 一个核验后的普通 Sub-task |
| [e2e-generate](skills/e2e-generate/SKILL.md) | 从计划生成测试，衔接独立执行 | 与 TC 对应的 Playwright spec |
| [e2e-heal](skills/e2e-heal/SKILL.md) | 诊断失败、修复测试、复验 | 修复结果，或可复现的问题记录 |

```text
skills/
├── e2e-workflow/
│   ├── SKILL.md
│   └── references/          计划模板、环境、定位、mock、可靠性与 CI
├── e2e-explore/
│   └── SKILL.md
├── e2e-sync-test-cases/
│   └── SKILL.md
├── e2e-generate/
│   └── SKILL.md
└── e2e-heal/
    └── SKILL.md
```

阶段 Skill 包含完整步骤、输入、输出与完成检查，共同读取 `e2e-workflow/references/`；不是指向另一大 Skill 的空入口。安装时将五个目录完整保留在同一 Skill 根目录，任务执行时按需调用。

## 工作顺序

```text
需求 / 验收 / 改动
       │
Phase 1    e2e-explore          → 测试计划
       ├─ 有 Jira 工单
       │   e2e-sync-test-cases  → 一个测试计划 Sub-task（按授权写入）
       └─ 无工单：跳过同步
       │
Phase 2    e2e-generate         → Playwright spec
       │
Phase 3    独立 runner 执行     → 报告与证据
       └─ 失败：e2e-heal       → 修复后复验
```

Phase 3 的执行方法保留在 [ci-execution.md](skills/e2e-workflow/references/ci-execution.md)，与原五 Skill 分工一致。

## 直接使用

> 用 `$e2e-workflow` 完成这次改动的 E2E 自动化。

> 用 `$e2e-explore` 探索这个需求，先交付测试计划。

> 用 `$e2e-generate` 按这份计划生成测试并执行。

> 用 `$e2e-heal` 定位这个失败用例，修复后复验。

客户端不支持 `$` 调用时，让 AI 读取同名目录下的 `SKILL.md`。一般功能验收、API 或单元回归仍由 [tester 角色](templates/agent.md) 按 [一般测试方法](docs/testing.md) 执行，不再经过聚合 Skill。

## 接入当前项目

```text
请读取 https://github.com/ai-sprites/tester 的 README 和接入手册，将 tester 角色与 e2e-workflow、e2e-explore、e2e-sync-test-cases、e2e-generate、e2e-heal 五个完整 Skill 接入当前项目。
固定同一明确版本，原样复制全部 SKILL.md 和参考文件，保持五个同级目录、名称和内部相对引用；只适配当前客户端的目录、元数据和入口。保留项目规则及用户自定义，不合并 Skill、不用摘要替代完整内容。
完成后核对实际文件、引用、采用版本和加载状态。资料来源和业务仓库已有选择就沿用，缺项合并问一次，可稍后配置。升级旧聚合版按接入手册完成替换与自定义保护。
```

[接入手册](docs/installation.md) 说明完整安装、升级与加载检查。具体项目的应用地址、账号、fixture、测试目录和 CI 配置从目标仓库读取。

## 产物与一般职责

计划、报告和必要证据保存到业务仓库 `docs/tester/`；测试代码、POM、fixture 和配置保留工程原目录。探索不等于执行通过，失败不能靠弱化断言隐藏；新增/修复用例需两次无重试隔离和一次无重试相关组验证。详见 [交付与留存](skills/e2e-workflow/references/delivery.md) 和 [产物交接](docs/artifact-handoff.md)。

一般测试的验收/风险映射、真实执行、缺陷证据和报告要求保留在 [docs/testing.md](docs/testing.md)，不另造笼统的测试 Skill。

## 完整资源

<details>
<summary>角色与共享参考清单</summary>

| 资源 | 用途 |
| --- | --- |
| [角色定义](templates/agent.md) | 职责、边界、Skill 选择 |
| [仓库约定](skills/e2e-workflow/references/repo-conventions.md) | 识别目标项目目录、fixture 和命令 |
| [认证与环境](skills/e2e-workflow/references/auth-and-environment.md) | 登录、网络、测试数据与隔离 |
| [探索工具](skills/e2e-workflow/references/exploration-tooling.md) | 浏览器、probe、debug 和 codegen |
| [计划模板](skills/e2e-workflow/references/plan-template.md) | 案例、步骤、预期与工单映射 |
| [选择器](skills/e2e-workflow/references/selectors-and-locators.md) | 稳定定位与控件断言 |
| [可靠性](skills/e2e-workflow/references/reliability-and-readiness.md) | 就绪顺序与八类常见失败 |
| [请求模拟](skills/e2e-workflow/references/request-mocking.md) | 窄范围路由和安全响应 patch |
| [CI 执行](skills/e2e-workflow/references/ci-execution.md) | 独立执行、复验与证据 |
| [交付与留存](skills/e2e-workflow/references/delivery.md) | 模板、目录、历史、索引与版本 |
| [测试报告模板](skills/e2e-workflow/references/test-report.md) | 正式执行报告 |

</details>

前八份共享参考保留原文件名；交付与报告参考承接 tester 已有职责。资源清单对应当前版本，安装时固定完整 Git 提交并读取同批文件。历史标签内容以对应标签为准；更新和移除按 [接入手册](docs/installation.md#已有内容与后续维护) 处理。
