# svn-review

专业级 SVN 代码审查工具 — Claude Code Skill

## 概述

svn-review 是一个运行于 [Claude Code](https://claude.com/claude-code) 的专业级代码审查 Skill。它模拟 15 年+ 经验的资深代码审查专家，对 SVN 变更进行全维度深度审查。

## 功能特性

### 七维度深度审查

| 维度 | 优先级 | 审查内容 |
|------|--------|---------|
| 安全性 Security | 🔴 最高 | SQL注入、XSS、命令注入、越权、敏感数据暴露 |
| 资源管理 Resource | 🔴 高 | 文件句柄、连接泄漏、内存管理 |
| 异常处理 Exception | 🔴 高 | 异常吞没、上下文丢失、错误传播 |
| 并发安全 Concurrency | 🔴 高 | 竞争条件、死锁、线程安全集合 |
| 兼容性 Compatibility | 🟠 中高 | API兼容、Schema变更、序列化兼容 |
| 性能 Performance | 🟡 中 | N+1查询、复杂度分析、索引利用 |
| 代码规范 Code Quality | 🔵 基础 | 命名、可读性、测试覆盖 |

### 专业级审查报告

- **四层次问题分析**：现象 → 根因 → 影响范围 → 工程化方案
- **兼容性专项评估**：独立章节，梳理所有兼容性风险
- **性能量化分析**：给出时间/空间复杂度，估算典型数据量影响
- **行动建议**：按 ROI 排序，帮助团队做优先级决策

## 使用方法

### 命令格式

```bash
# 审查本地未提交改动 vs HEAD
svn-review

# 审查本地改动 vs 指定版本 N
svn-review --base rN

# 审查版本 M 到版本 N 的差异
svn-review --from rM --to rN

# 限定审查范围到特定目录
svn-review --path src/core

# 指定 SVN 仓库地址
svn-review --url https://svn.example.com/repo

# 输出英文报告
svn-review --lang en

# 快速扫描模式（只报告 P0/P1）
svn-review --quick
```

参数可组合使用，例如：
```bash
svn-review --from r200 --to r210 --path src/core
```

### 模糊触发

即使没有明确的命令，以下模糊表达也会自动触发审查：

- "帮我看看改了什么"
- "这次改动有没有问题"
- "审查SVN代码"
- "检查本地改动"
- "review svn变更"

## 安装

### 方法一：直接使用（推荐）

将 `SKILL.md` 文件内容复制到 Claude Code 的自定义 Skill 配置中。

### 方法二：插件安装

1. 克隆本仓库到本地
2. 在 Claude Code 中配置 Skill 路径指向本目录

## 报告示例

审查完成后，自动生成结构化 Markdown 报告，包含：

```markdown
# SVN 代码审查报告

**审查时间**：2026-04-03 10:30
**审查范围**：r53207 → r53472
**风险等级**：🟠 中危
**发现问题**：P0:0 · P1:2 · P2:3 · P3:5

---

## 📊 变更概览

| 指标 | 数值 |
|------|------|
| 变更文件数 | 15 |
| 新增代码行 | +328 |
| 删除代码行 | -156 |

## 🔗 兼容性专项评估

[独立章节，详细评估兼容性风险]

## 🚨 问题列表

[P0/P1/P2/P3 分级展示，带根因分析和修复方案]

## 📋 行动建议

[按 ROI 排序的修复任务清单]
```

报告自动保存到 `docs/code-review/` 目录。

## 问题优先级

| 级别 | 触发条件 | 处理要求 |
|------|---------|---------|
| **P0** | 安全漏洞可直接利用 / 数据丢失 / 服务不可用 | 阻塞合入，必须修复 |
| **P1** | 高并发/边界条件下必然触发的崩溃、泄漏、数据不一致 | 强烈建议本次修复 |
| **P2** | 正常使用中偶发问题，或性能在百万级以上明显退化 | 建议近期版本跟进 |
| **P3** | 不影响功能正确性的规范/可读性问题 | 可选优化 |

## 项目结构

```
svn-review/
├── SKILL.md                      # Skill 主文件（Claude Code 配置文件）
├── README.md                     # 项目文档
├── LICENSE                       # 开源许可证
├── .gitignore                    # Git 忽略配置
└── references/
    └── review-dimensions.md      # 七维度详细审查清单
```

## 技术细节

- **适用场景**：企业级 SVN 仓库代码审查
- **支持语言**：Java、Python、Go、JavaScript/TypeScript、C/C++ 等
- **依赖工具**：SVN CLI (`svn` 命令)
- **输出格式**：Markdown 结构化报告

## 维护

本 Skill 会随项目使用经验持续更新，如发现常见问题未被覆盖，欢迎提交 Issue 或 Pull Request。

## 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件
