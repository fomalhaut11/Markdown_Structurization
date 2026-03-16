# project-ai-template

一套面向 AI 辅助开发的项目文档模板。让 LLM（Claude、Codex 等）高效理解你的项目并参与开发维护。

## 设计理念

**核心问题**：LLM 在参与项目开发时，缺乏对项目架构、模块边界和数据流的系统认知，导致生成的代码容易偏离项目约定。

**解决思路**：用结构化文档为 LLM 提供"项目心智模型"——

1. **CLAUDE.md** 作为常驻上下文（< 20 行），只放绝对约束和入口指引
2. **docs/** 作为按需加载的知识库，LLM 在需要时主动查阅
3. **skills/** 定义标准化工作流程，让 LLM 按角色、按步骤执行任务

这套模板工具无关，适用于任何支持 Markdown 的 AI 编码助手。

## 快速开始

### 1. 复制模板到你的项目

```bash
cp -r project-ai-template/* your-project/
cp -r project-ai-template/.claude your-project/
```

### 2. 填写 CLAUDE.md

打开 `CLAUDE.md`，替换 `[占位符]` 为你的项目信息：
- 语言/框架
- 包管理器和常用命令
- 项目级约束

### 3. 填写架构文档

打开 `docs/architecture.md`，填写：
- 技术栈
- 目录结构
- 模块列表和依赖关系
- 通信模式

### 4. 为每个核心模块创建文档

复制 `docs/modules/_module-template.md` 并重命名：

```bash
cp docs/modules/_module-template.md docs/modules/auth.md
cp docs/modules/_module-template.md docs/modules/order.md
```

填写每个模块的职责、公开接口、依赖关系。

### 5. 记录关键数据流

打开 `docs/data-flows.md`，为每个核心业务流程记录跨模块调用链。

## 文件说明

| 文件 | 用途 | 加载方式 |
|------|------|----------|
| `CLAUDE.md` | 绝对约束 + 入口指引 | 常驻上下文（自动加载） |
| `docs/architecture.md` | 项目全景 | 按需加载 |
| `docs/modules/*.md` | 模块接口文档 | 按需加载 |
| `docs/data-flows.md` | 跨模块数据流 | 按需加载 |
| `.claude/skills/*/SKILL.md` | 标准化工作流 | 按角色触发 |

## Skills 一览

| Skill | 角色 | 用途 |
|-------|------|------|
| understand-project | 项目分析师 | 首次接触项目时建立全景认知 |
| add-feature | 功能开发者 | 按规范流程添加新功能 |
| fix-bug | 调试专家 | 系统化定位和修复缺陷 |
| review-code | 代码审查员 | 审查代码变更 |
| update-docs | 文档维护者 | 保持文档与代码一致 |

## 设计原则

- **常驻上下文最小化**：CLAUDE.md 控制在 20 行以内，避免浪费 token
- **按需加载**：docs/ 下的文档只在需要时读取
- **渐进式披露**：skill frontmatter 精简，正文按步骤展开
- **工具无关**：不依赖特定 AI 工具的私有功能
- **占位符标注**：所有需要用户填写的部分用 `[占位符]` 标注

## 维护建议

- 代码变更后及时更新对应文档（可使用 update-docs skill）
- 定期检查文档与实现的一致性
- 新增模块时同步创建模块文档
- 数据流文档重点覆盖核心业务路径，不必穷举所有流程
