# 产物留存与交接

功能验证、回归或验收要保存真实测试报告，包含被测版本、判定、复现证据和未覆盖项。简单检查可在回复中说明；未运行不能写成通过。
具体要求、完整模板和保存规则见[本职 Skill](../skills/sprite-tester-verification/SKILL.md)。

上游资料仓库、版本和文件，与本次产物的业务仓库分别选择，可以不同；上游资料按其实际位置读取。
沿用已明确的仓库选择，只问影响当前工作的缺项；暂时不需要的可稍后配置。产物目录固定为选定业务仓库的 `docs/tester/`。

本角色生成的测试报告、复现记录和验证证据（含日志、截图及附件）统一保存到业务仓库的 `docs/tester/`，可按功能在角色目录下再建子目录，例如 `docs/tester/<feature-id>/verification.md`。新增和更新产物均遵守此路径；已有散落产物先读取，在保留内容的前提下迁入角色目录，目标同名文件不得直接覆盖，迁移后同步相关引用和已有索引。产品代码、测试代码及测试配置仍使用工程原生目录，角色与 Skill 安装资源保持原位。上游资料可从实际所在位置读取。

本地文件直接使用，不需要 bridge，也不要求接收方安装其他角色。
实际需要跨 Git 读取、固定版本或导出时，复用 bridge；缺失或能力不足时按[接入手册](https://github.com/ai-sprites/artifact-bridge/blob/main/docs/installation.md)自动补齐完整、同一明确版本的 Skill，适配当前客户端、保留自定义并告知用户。

交接时核对：

- **文件齐全：**正文和必要附件逐个选入。链接不会自动展开，目录不会递归读取；外链只留下地址，需要留存其内容时另行导出并登记实际文件。
- **路径正确：**Markdown 链接相对所在文档，bridge 索引的文件路径相对资料 Git 根。索引按需使用；已有索引随文件变化同步，保留其他条目。
- **版本可取：**每个来源的正文、索引和附件须进入选定且接收方可获取的提交。在已有授权内完成 Git 协作；bridge 不替上游提交或推送，也不自动追踪新版本。
- **结果真实：**核对实际读取与导出结果中的完整性、错误和逐项状态。bridge 原始导出包（含资料副本、附件及 `manifest.json`）统一保存在业务仓库的 `docs/artifact-bridge/` 下，可按功能放入 `docs/artifact-bridge/<feature-id>/`；默认目录为 `docs/artifact-bridge/<sourcehash>/<selectionhash>/<commit>/`。本角色据此生成的报告和加工附件仍保存到 `docs/tester/`。保留原来源映射，包含仓库、完整提交、源文件与留存位置。交付真实目录，缺文件、超限、外链等未留存项单列说明。
- **下一步明确：**给出实际文件链接，区分已保存、已提交、远端可获取和已导出。不能落盘或访问时如实说明；需要用户提供来源、选择业务仓库、登录或补授权，就直接提出具体问题，继续不受影响的工作。

命令与导出规则见 [bridge 读取与留存](https://github.com/ai-sprites/artifact-bridge/blob/main/skills/sprite-artifact-bridge/references/commands.md)，索引格式见[索引与角色交接](https://github.com/ai-sprites/artifact-bridge/blob/main/skills/sprite-artifact-bridge/references/index.md)。使用当前采用版本的完整说明。
