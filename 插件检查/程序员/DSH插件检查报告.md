# DSH 插件检查报告

- 检查时间：2026-11-08 21:00–21:10（Asia/Shanghai）
- 本机 DSH 运行时版本：`0.2.2-rc.1`，Node `v26.10.0`，npm `11.19.1`
- 检查方式：`plugin_manager`（list_plugins / list_bundles / list_version_exemptions）+ GitHub API + npm registry
- 结论摘要：**1 个插件加载失败；2 处功能重叠可关注；4 个插件有更新，其中 1 个为大版本迁移建议人工确认；无版本豁免、无 DSH 版本告警。**

---

## 一、GitHub 好用插件推荐清单

来源：GitHub 仓库搜索（`DeepSeek Harness plugin`、`topic:dsh-plugin` 按 star 排序），检索日期 2026-11-08。

### 精选索引 / 入口（先装这个找插件）

| 名称 | 地址 | 用途 | 亮点 |
| --- | --- | --- | --- |
| awesome-dsh-plugin | https://github.com/awesome-dsh-plugin/awesome-dsh-plugin | DSH 插件精选列表 | 23.7k★，最权威的中文/英文双语精选清单，持续更新 |
| imsai-sh/awesome-deepseek-harness-plugins | https://github.com/imsai-sh/awesome-deepseek-harness-plugins | 插件商店 / 市场目录 | 11,000+ 插件自动收录，带搜索 API（deepseek1024.com）与安装命令 |
| 0xsline/awesome-deepseek-harness | https://github.com/0xsline/awesome-deepseek-harness | DSH 生态精选 | 1.3k★，插件 + 工具 + 基础设施，源码核验 |
| libukai/awesome-deepseek-harness | https://github.com/libukai/awesome-deepseek-harness | 终极指南 | 快速入门 + 精选插件 + 实用工具，适合新人 |

### 高星功能插件

| 名称 | 地址 | 用途 | 亮点 |
| --- | --- | --- | --- |
| dsh-market | https://github.com/dsh-market/dsh-market | 可视化插件市场 | 11.5k★，界面内浏览、搜索、一键安装社区插件（本机已装同类 dshmarket） |
| zhu1090093659/dsh-web | https://github.com/zhu1090093659/dsh-web | Web 插件聚合生态 | 10.2k★，创意工坊分发，Web UI 类插件集合 |
| Devin-AXIS/deepseek-design | https://github.com/Devin-AXIS/deepseek-design | 设计系统 + PPT 工作室 | 1.8k★，AI 生成、可视化编辑、模板市场 |
| MeteorNOX/DeepSeek-Balance-Whale-Widget | https://github.com/MeteorNOX/DeepSeek-Balance-Whale-Widget | 余额监控挂件 | 7.0k★，右下角鲸鱼挂件盯 DeepSeek 账户余额，拖拽吸附 |
| liguobao/ds-harness-remote | https://github.com/liguobao/ds-harness-remote | 远程访问 | 2.2k★，端到端加密 P2P，手机/Web 访问本地 DSH 工作区 |
| Alice-Marx/dsh-model-router | https://github.com/Alice-Marx/dsh-model-router | 模型路由 | 900★，多模型切换路由 |
| liangmianya/dsh-synapse | https://github.com/liangmianya/dsh-synapse | 非线性会话画布 | 686★，画布式会话浏览与分支工作区 |
| xuanyuanzhifeng/dsh-plugin-agent-workflow | https://github.com/xuanyuanzhifeng/dsh-plugin-agent-workflow | Agent 工作流 | 321★，TS 编写，流程编排 |
| Ebony-Vinyl/dsh-our-free-model | https://github.com/Ebony-Vinyl/dsh-our-free-model | 免费模型接入 | 66k★，免登录免 Key 接入多个前沿模型——**注意：第三方免费中转，隐私与稳定性风险自负，装前审阅其数据去向** |
| dsh-tauri/deepseek-harness-desktop | https://github.com/dsh-tauri/deepseek-harness-desktop | Tauri 桌面壳 | 3.5k★，零环境配置桌面版（非插件，属生态工具） |

> 备注：搜索结果中还有 rea（442k★）、open-design、archify 等超星仓库，均非 DSH 专属插件（仅打了 dsh-plugin 标签的通用 agent 工具），未列入推荐。

---

## 二、本机已装插件冲突与异常排查

数据：`list_plugins` 共 **250 条 entry**，`list_bundles` 共 **33 个 bundle**（含未安装的官方 profile）。

### 异常（需处理）

1. **`@deepseek-ai/dsh-experimental-inspector` 加载失败**
   - entryId：`include:experimental-inspector`，`enabled: true` 但 `fiberPhase: "failed"`
   - 诊断信息：`inspector: globalThis.fetch is an accessor and cannot be observed safely`
   - 影响：官方实验性 Inspector（原始 Session 日志 / Chat 节点检查）不可用；同 bundle 的 `session-inspector`、`client-ui-stats` 正常 active，不波及其他功能。
   - 建议：在设置中关闭 experimental-inspector-profile bundle，或等上游修复；**未自行处理**（属官方实验功能，关闭与否请示）。

### 功能重叠（可关注，非报错）

2. **两个插件市场并存**：官方 `@deepseek-ai/dsh-plugin-market`（`include:plugin-market` active）与社区 `dshmarket` 1.73.6（`include:dsh-market` active）同时启用，入口功能重叠，可能在 UI 出现两处"市场"。可保留其一。
3. **session-query 模块重复注册**：`include:session-query-sqlite`（base，active）与 `include:optional-session-query-sqlite`（session-search bundle，active）指向同一模块 `@deepseek-ai/dsh-session-query-sqlite`，双份加载。属官方 bundle 叠加行为，当前无报错，留意即可。
4. **官方 terminal bundle 行配置笔误**：`optional-terminal-pwsh` 行的 moduleName 是 `@deepseek-ai/dsh-terminal-bash`（与 bash 行同模块），pwsh 行实际启用、bash 行被禁用。Windows 下 pwsh 常用，当前工作正常；如需常驻 bash 终端需留意此坑。

### 正常状态说明（非异常）

- 大量官方 entry `enabled: false`（`tool-bash`、`tool-fs`、`tool-skill`、`compaction-basic`、`plan-mode`、`tool-web`、`tool-subagent*` 等）均因被第三方 bundle **overrides 接管**：`dsh-ops` 接管文件/PTC 工具、`billion-context` 接管 compaction、`ds-bot` 接管 web、`agent-team`/`clearai` 接管 subagent 与 preset。与 `list_bundles` 的 overrides 列表逐一对应，属预期替换链，不是冲突。
- `@liustack/modlens` bundle 已安装但 `enabled: false`（用户侧停用），无异常。
- 所有启用 entry 中除 inspector 外 `fiberPhase` 全部为 `active`，无其他失败。

---

## 三、更新检查

`list_version_exemptions`：`runtimeVersion 0.2.2-rc.1`，`exemptions: {}`，`warnings: []` —— **无版本豁免、无升级告警**。

### 有更新的插件（4 个）

| 插件 | 已装 | 最新 | 更新要求核验 | 建议 |
| --- | --- | --- | --- | --- |
| dshmarket | 1.73.6 | 1.73.15 | peer `dsh-settings ^0.2.1-rc.1` 覆盖 0.2.2-rc.1 ✓ | 可更新（patch 级） |
| billion-context | 0.1.212 | 0.1.213 | engines `node>=20`（本机 26）✓ | 可更新（patch 级，注意它接管 compaction，更新后验证压缩行为） |
| clearai-dsh | 0.5.9 | 0.6.0 | engines `dsh>=0.2.2-rc.1` ✓、`node>=22` ✓ | **minor 升级且含 preset 变更，建议确认 changelog 后再升** |
| @liustack/modlens | 3.27.2 | 3.27.3 | engines `node>=22.19` ✓ | bundle 当前已停用，优先级低 |

### 需人工确认的大版本（1 个）

- **dsh-mnemosyne 2.0.0（git main 安装）→ 上游 v3.0.0**
  - 上游 tag `v3.0.0` 已发布（package.json version 3.0.0，附 `MIGRATION_3.0.md`），本机停在 2.0.0。
  - 注意：npm registry 上 `dsh-mnemosyne` 的 latest 为 0.7.0，与 git tag 体系不一致（该插件以 git 源安装），以 GitHub tag 为准。
  - **属大版本迁移，按归档规范不自行升级、不自行豁免，仅报告。**

### 已是最新（无需动作）

dsh-better-sidebar 0.30.1、dsh-univer-office 0.4.0、ds-bot 0.3.1、dsh-ops 0.5.1；官方 `@deepseek-ai/*` 全家桶均 0.2.2-rc.1（与运行时同版本）。

---

## 四、升级风险提示

- 本机运行时为 `0.2.2-rc.1`（rc 阶段），所有升级都建议在空闲会话期进行，升级后跑一轮冒烟：正常对话、bash/文件工具、压缩（billion-context）、市场入口。
- 涉及崩溃/数据丢失风险的操作（dsh-mnemosyne 3.0 迁移、clearai 0.6.0）**已按要求只报告不执行**；若需豁免版本检查，须用户明确批准后再由 plugin_manager 授予。
