# cursor-rules-ios

> 面向 iOS 项目的 Cursor 规则与 Serena 项目记忆集合，用于规范 AI 协作开发流程，并提供跨项目可复用的工作流与工具指引。

## 项目简介
- **定位**: 本仓库不是 App 源码，而是提供在 Cursor 中进行 iOS 开发时的规则、流程与知识库。
- **目标**:
  - 将「通用规则」与「项目知识」分层管理，提升 AI 协作效率与一致性。
  - 支持一键复用到任意 iOS 项目，降低新项目启用成本。
- **适用范围**: 所有使用 Swift/UIKit 的 iOS 项目（示例记忆以借贷类 App 为模板）。

### 维护与更新声明
- 本仓库中的规则与记忆模板均为我在实际项目中长期使用与打磨的实践沉淀；
- 后续将不定期更新，以适配新版本工具链与更优工作流；
- 文档旨在为大家在构建自有规则体系时提供可复用的思路与参考实现。

## 目录结构
```
.cursor/rules/                      # Cursor 规则（规范、流程、工具）
├── 00-personal.mdc                 # 个人编码偏好（Always）
├── 01-serena-workflow.mdc          # Serena 工作流程（Always）
├── 02-project-conventions.mdc      # 项目约定（Always，需按项目填写）
├── 03-project-plan-sync.mdc        # Plan 文档同步规则（Always）
├── README.md                       # 规则说明（当前目录说明）
├── tools/                          # 工具类规则（按需触发）
│   ├── plan-reference.mdc         # Plan 文档参考样式
│   ├── problem-handling.mdc        # 问题处理合集
│   ├── quick-build.mdc             # 快捷构建
│   ├── rules-maintenance.mdc       # 规则编写指南
│   ├── serena-maintenance.mdc      # Serena 记忆维护
│   └── troubleshooting-log.md      # 故障排查日志
└── XcodeMCP/                       # Xcode MCP 工具（按需触发）
    ├── xcode-build.mdc             # Xcode 构建运行
    └── xcode-log.mdc               # 设备/模拟器日志

.serena/memories/                   # Serena 项目记忆（项目知识库）
├── project_overview.md             # 项目概览（必须）
├── tech_stack.md                   # 技术栈细节（推荐）
└── common_patterns.md             # 常见模式与示例（推荐）
```

## 使用方式（在 Cursor 中）
- **核心规则自动加载**（Always）
  - `00-personal.mdc`、`01-serena-workflow.mdc`、`02-project-conventions.mdc`、`03-project-plan-sync.mdc`
- **项目任务场景**（自动触发 Serena 工作流）
  1) 初始化 Serena：加载 `project_overview.md`
  2) 需要第三方库/实现模式：再加载 `tech_stack.md`、`common_patterns.md`
  3) 探索代码：优先使用符号化工具（禁止直接全文 `read_file`）
- **工具规则按需触发**：
  - **构建运行**：`@quick-build`、`@xcode-build`
  - **日志分析**：`@xcode-log`
  - **维护工具**：`@serena-maintenance`、`@rules-maintenance`
  - **问题处理**：`@problem-handling`
  - **Plan 文档**：`@plan-reference`（编写/更新 plan 文档时）

### 关于记忆的生成与维护
- `.serena/memories` 下的记忆文件由 `@serena-maintenance` 基于你的项目自动生成与持续维护；
- 你只需在对话中触发 `@serena-maintenance`（或按 README 指引操作），AI 会读取并使用这些记忆进行协作；
- 当项目有结构/技术栈变更时，重新运行 `@serena-maintenance` 以更新记忆内容。

### 关于 Plan 文档同步
- `03-project-plan-sync.mdc` 确保 `项目.plan.md` 与代码保持同步；
- 代码修改后自动检查是否需要更新 plan 文档（架构变更、功能变更、UI 变更等）；
- 生成代码前会检查 plan 文档中的架构规范，确保复用现有组件。

## 项目记忆模板（示例）
- `project_overview.md` 提供了一套借贷类 App 的模板信息（模块划分、网络层、基类、配置等），可作为新项目初始化参考。
- 如需落地到你的真实项目，建议：
  - 完善 `项目名称/Bundle ID/目录结构/关键约定` 等字段；
  - 用你的实际第三方库替换 `tech_stack.md` 中的示例；
  - 把通用的控制器基类/网络层模式沉淀到 `common_patterns.md`。

## 快速开始（复用到新项目）
1. 复制 `.cursor/rules/` 至你的项目根目录。
2. 根据你的项目，填写 `02-project-conventions.mdc` 与搭建 Serena MCP。
3. 在 Cursor 中直接开始对话，触发 `@serena-maintenance` 自动生成/更新记忆文件。
4. 如需使用 Plan 文档同步功能，确保项目根目录有 `项目.plan.md` 文件。

## 与 iOS 开发的关系
- 本仓库不包含业务代码，但内置的规则/记忆对以下方面提供指导：
  - 架构与基类组织（如 `SSBaseViewController`、`SSBaseRequest` 等模板思路）
  - 网络请求/缓存/异步流程的通用约定
  - 构建、运行、日志等 Xcode 工具化动作
  - Plan 文档与代码的双向同步机制

## 贡献指南
- 提交前请确保：
  - 新增/修改的规则清晰、可复用、与现有体系一致；
  - Always 规则保持精简；
  - 工具规则提供清晰的触发方式与示例。
- 建议在 PR 描述中附：变更动机、适用场景、示例对话。

## 版本与维护
- 版本历史与变更要点请参考 `.cursor/rules/README.md` 中的「版本历史」。
- 建议定期运行 `@serena-maintenance` 对记忆文件进行审计与更新。
- 规则优化请参考 `@rules-maintenance` 指南，确保规则的可执行性和有效性。

## 许可证
- 若无特别说明，文档与规则以 MIT 许可发布。你可以自由复用并在项目内修改。

---
如需根据你的业务进行定制，可在 `02-project-conventions.mdc` 与 `.serena/memories/*` 中补充你项目的专属信息。
