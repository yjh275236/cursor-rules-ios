# Cursor Rules 说明

本目录包含 **通用的** AI 辅助开发规则配置，可在多个项目间复用。

> 💡 **特别说明**: 所有规则中的 `{项目名}` 为占位符，AI 会根据当前激活的 Serena MCP 服务器自动识别并替换。

## 核心原则

**alwaysApply: true → 简洁**
- 始终加载，影响所有对话
- 节省 token，快速加载
- 只保留核心要点

**alwaysApply: false → 详细**
- 按需手动触发（使用 `@规则名`）
- 可包含详细模板和示例
- 作为完整参考指南

---

## 文件结构

```
.cursor/rules/
├── 00-personal.mdc              # 个人编码偏好 (always)
├── 01-serena-workflow.mdc       # Serena 工作流程 (always)
├── 02-project-conventions.mdc   # 项目约定 (always)
├── 03-problem-handling.mdc      # 问题处理指南 (always)
├── README.md                    # 本文件
└── tools/                       # 工具类规则 (按需触发)
    ├── quick-build.mdc          # 快捷构建命令
    ├── serena-maintenance.mdc   # Serena 维护指南
    ├── xcode-build.mdc          # Xcode 构建工具
    ├── xcode-log.mdc            # Xcode 日志工具
    └── xcode-ui.mdc             # Xcode UI 自动化
```

---

## 核心规则（根目录）

### 1. `00-personal.mdc`（~40 行）✅ Always

**用途**: 个人编码偏好和 AI 协作风格（**通用，可跨项目复用**）

**内容**:
- 回答格式要求（说明使用的规则/记忆/MCP）
- 📋 强制检查清单
  - 步骤 1：初始化 Serena MCP（`initial_instructions`, `check_onboarding`, `list_memories`）
  - 步骤 2：代码探索优先级（🚨 禁止直接 `read_file` 读代码）
- 编码规范
  - 工作方式（先思考再实现、逐步推理、简明扼要）
  - 代码质量（完整性、可读性、安全性）

**跨项目使用**: 直接复制到新项目的 `.cursor/rules/` 目录
  - 诚信原则（明确不确定性、不猜测）

**可复用性**: ✅ 完全可跨项目复用

---

### 2. `01-serena-workflow.mdc`（~65 行）✅ Always

**用途**: 指导 AI 如何使用 Serena MCP 的 memory 功能

**内容**:
- 🎯 执行条件（自动判断技术代码指标）
- ⚡ 强制步骤（必须执行的加载流程）
- ✅ 自我验证（回复前确认检查清单）
- 📚 标准文件清单（必须/推荐两级分类）

**可复用性**: ✅ 完全可跨项目复用

---

### 3. `02-project-conventions.mdc`（~6 行）✅ Always

**用途**: SeasysStart 项目特定约定

**内容**: 当前为空，可填充：
- 项目标识（名称、前缀、Bundle ID）
- 技术栈（简洁列表）
- 代码约定（要点）
- 目录结构（简化版）
- 命名规范

**可复用性**: ⚠️ 需要修改为新项目的约定

---

### 4. `03-problem-handling.mdc`（~10 行）✅ Always

**用途**: 问题处理指南

**内容**:
- AI 在问题处理中的职责（提供思路，不直接解决）
- 问题处理流程
- Troubleshooting Memory 由用户维护的说明

**可复用性**: ✅ 完全可跨项目复用

---

## 工具规则（tools/ 目录）

### 5. `tools/quick-build.mdc`（~51 行）❌ 手动触发

**用途**: 快捷构建和运行命令

**触发方式**: `@quick-build`

**功能**:
- 自动检测项目信息（workspace/scheme）
- 重启模拟器
- 静默构建应用
- 安装并启动应用

**可复用性**: ✅ 通用 iOS 项目可复用

---

### 6. `tools/serena-maintenance.mdc`（~930 行）❌ 手动触发

**用途**: 构建和维护 Serena memories

**触发方式**:
```
@serena-maintenance                    # 智能模式（AI 自动判断）
@serena-maintenance 从头构建 memories   # 手动指定
@serena-maintenance 更新 tech_stack    # 更新特定文件
@serena-maintenance 审计质量            # 质量检查
```

**内容**:
- 智能使用模式（AI 自动判断操作）
- Memory 文件结构规范
- 完整标准模板（通用化）
- 构建流程和质量审计

**可复用性**: ✅ 完全可跨项目复用（已通用化）

---

### 7. Xcode 工具集（~285 行）❌ 手动触发

#### `tools/xcode-build.mdc`（~57 行）
- 构建、运行、测试 iOS 应用
- 支持模拟器和真机
- 触发方式: `@xcode-build`

#### `tools/xcode-log.mdc`（~97 行）
- 捕获和分析设备/模拟器日志
- 实时日志监控
- 触发方式: `@xcode-log`

#### `tools/xcode-ui.mdc`（~181 行）
- UI 自动化测试
- 截图和 UI 元素检查
- 触发方式: `@xcode-ui`

**可复用性**: ✅ 通用 iOS 项目可复用

---

## 职责划分

### Cursor Rules（本目录）
- ✅ 个人偏好和编码规范
- ✅ Serena 使用指南（简洁）
- ✅ 项目特定约定（简洁列表）
- ✅ 问题处理指南（提供思路）
- ✅ Memory 维护指南（详细，按需加载）
- ✅ 开发工具指南（Xcode、快捷命令等）

### Serena Memories（`.serena/memories/`）
- ✅ 项目详细信息（project_overview.md）
- ✅ 技术栈使用详情（tech_stack.md）
- ✅ 代码模式和示例（common_patterns.md）
- ❌ Troubleshooting 由用户手动维护

**关键区别**: Cursor = 规范和指导，Serena = 项目知识

---

## 规则体系优势

1. **简洁高效**: Always 规则 ~121 行，不占用过多 token
2. **智能触发**: 自动识别项目任务，强制加载 Serena memories
3. **详细指导**: 工具规则 ~1316 行，提供完整参考
4. **清晰分层**: 核心规则（根目录）vs 工具规则（tools/）
5. **高复用性**: 80% 的规则可直接复用到新项目
6. **易于维护**: 职责清晰，结构分明
7. **按需加载**: 工具规则仅在需要时触发

---

## 应用到新项目

### 完全复用（无需修改）
- ✅ `00-personal.mdc`
- ✅ `01-serena-workflow.mdc`
- ✅ `03-problem-handling.mdc`
- ✅ `tools/` 目录下所有工具规则

### 需要修改
- ⚠️ `02-project-conventions.mdc`: 更新为新项目的约定
  - 项目名称和 Bundle ID
  - 类名前缀
  - 技术栈
  - 目录结构
  - 命名规范

### 重新构建
使用 `@serena-maintenance` 为新项目构建 Serena memories（智能模式）

---

## 文件大小统计

### 核心规则（Always）
| 文件 | Always | 行数 |
|------|--------|------|
| `00-personal.mdc` | ✅ | ~40 行 |
| `01-serena-workflow.mdc` | ✅ | ~65 行 |
| `02-project-conventions.mdc` | ✅ | ~6 行 |
| `03-problem-handling.mdc` | ✅ | ~10 行 |
| **核心规则总计** | | **~121 行** |

### 工具规则（按需）
| 文件 | Always | 行数 |
|------|--------|------|
| `tools/quick-build.mdc` | ❌ | ~51 行 |
| `tools/serena-maintenance.mdc` | ❌ | ~930 行 |
| `tools/xcode-build.mdc` | ❌ | ~57 行 |
| `tools/xcode-log.mdc` | ❌ | ~97 行 |
| `tools/xcode-ui.mdc` | ❌ | ~181 行 |
| **工具规则总计** | | **~1316 行** |

**总计**: ~1437 行
- Always 规则: ~121 行（精简高效，占 8%）
- 工具规则: ~1316 行（详细完整，占 92%）

---

## 版本历史

### v3.2 - 2025-10-30
**主题**: 规则通用化与 Serena MCP 强制使用

- ✅ **规则通用化改造**
  - 移除硬编码项目名称（`next1` → `{项目名}`）
  - AI 自动检测 Serena MCP 服务器名称
  - 规则可跨项目直接复用
- ✅ **强制 Serena MCP 使用**
  - 新增"步骤 3：使用 Serena MCP 工具探索代码"
  - 🚨 严格禁止直接 `read_file` 读取代码文件
  - 明确探索阶段与编辑阶段的工具选择
- ✅ **新增工具决策树**
  - 清晰的场景 → 工具映射表
  - 标注禁止使用的工具
  - 提供正确/错误工作流示例
- ✅ **完善初始化流程**
  - 新增 `initial_instructions` 和 `check_onboarding_performed`
  - 明确 6 步初始化检查清单
- ✅ **自我验证增强**
  - 新增"是否优先使用符号化工具"检查项
  - 检查清单从 4 项扩展到 7 项

**改进动机**: 用户反馈 AI 未充分使用 Serena MCP，直接使用 `read_file` 和 `grep` 导致效率低下

### v3.1 - 2025-10-29
**主题**: 强化规则触发机制

- ✅ 增强 `00-personal.mdc`
  - 新增 📋 强制检查清单（项目任务自动触发）
  - 简化检查流程（只保留核心项目上下文加载）
- ✅ 重写 `01-serena-workflow.mdc`
  - 新增 🎯 执行条件（明确的技术代码指标判断）
  - 新增 ⚡ 强制步骤（命令式语言，必须执行）
  - 新增 ✅ 自我验证（回复前确认机制）
- ✅ 更新 README.md（反映规则简化）

### v3.0 - 2025-10-28
**主题**: 强化编码规范，完善文档体系

- ✅ 扩展 `00-personal.mdc` 编码规范
  - 新增工作方式指导（先思考再实现、逐步推理）
  - 新增代码质量要求（100%完整、可读性优先）
  - 新增诚信原则（明确不确定性、不猜测）
- ✅ 重构 README.md
  - 优化文档结构和层次
  - 完善工具规则说明
  - 新增使用建议章节

### v2.0 - 2025-10-24
**主题**: 规则体系重构与职责划分

- ✅ 确立核心原则（Always 简洁 / 按需详细）
- ✅ 规则文件组织
  - 核心规则 4 个（~94 行，始终加载）
  - 工具规则移至 `tools/` 子目录（~1316 行，按需触发）
- ✅ 职责清晰化
  - Cursor Rules：规范和指导
  - Serena Memories：项目知识
- ✅ 新增 `03-problem-handling.mdc`（AI 提供思路，不直接解决）
- ✅ Serena Maintenance 智能化（支持自动判断操作）

### v1.0 - 2025-10-20
**主题**: 初始化规则体系

- ✅ 创建基础规则文件
  - `00-personal.mdc`：个人偏好配置
  - `01-serena-workflow.mdc`：Serena 工作流程
  - `02-project-conventions.mdc`：项目约定
- ✅ 引入 Serena MCP 支持
- ✅ 创建工具规则
  - Xcode 工具集（build/log/ui）
  - Quick Build 快捷命令
  - Serena Maintenance 维护指南
- ✅ 建立 README.md 文档体系

---

## 🔄 跨项目复用指南

### 如何将规则迁移到新项目

1. **复制通用规则文件**（可直接使用，无需修改）
   ```bash
   # 复制到新项目
   cp -r .cursor/rules/00-personal.mdc /path/to/newproject/.cursor/rules/
   cp -r .cursor/rules/01-serena-workflow.mdc /path/to/newproject/.cursor/rules/
   cp -r .cursor/rules/README.md /path/to/newproject/.cursor/rules/
   ```

2. **创建项目特定规则**（根据新项目调整）
   ```bash
   # 创建新项目约定文件
   touch /path/to/newproject/.cursor/rules/02-project-conventions.mdc
   ```

3. **配置 Serena MCP**
   - 确保新项目已配置 Serena MCP 服务器
   - 工具会自动检测服务器名称（如 `mcp_serena-myapp_*`）
   - 无需修改规则文件中的任何内容

4. **初始化项目记忆**
   - 使用 `@serena-maintenance` 创建 `project_overview.md`
   - 可选：创建 `tech_stack.md`, `common_patterns.md`

### 通用规则清单

✅ **完全通用**（零修改可复用）:
- `00-personal.mdc` - 个人编码偏好
- `01-serena-workflow.mdc` - Serena MCP 工作流
- `README.md` - 规则说明文档
- `tools/` 目录下的所有工具规则

⚠️ **需要定制**（根据项目调整）:
- `02-project-conventions.mdc` - 项目特定约定（类名前缀、架构模式等）

---

## 使用建议

### 日常开发
- 核心规则自动加载，无需手动触发
- 编码时遵循 `00-personal.mdc` 的规范

### 快速构建
```
@quick-build  # 重启模拟器并运行
```

### 维护 Memories
```
@serena-maintenance  # 智能模式，AI 自动判断需要什么操作
```

### Xcode 开发
```
@xcode-build  # 构建和运行
@xcode-log    # 查看日志
@xcode-ui     # UI 测试
```

---

**维护原则**: 
- Always 规则保持精简（<150 行）
- 强制触发机制确保项目任务自动加载 memories
- 工具规则可以详细（提供完整参考）
- 定期审查和更新，保持规则与项目同步
