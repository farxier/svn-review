# svn-review

Professional SVN Code Review Tool — Claude Code Skill

## Overview

svn-review is a professional code review Skill running on [Claude Code](https://claude.com/claude-code). It simulates a senior code reviewer with 15+ years of experience, performing full-dimensional deep review of SVN changes.

## Features

### Seven-Dimensional Deep Review

| Dimension | Priority | Coverage |
|-----------|----------|----------|
| Security | 🔴 Highest | SQL injection, XSS, command injection, authorization bypass, sensitive data exposure |
| Resource Management | 🔴 High | File handles, connection leaks, memory management |
| Exception Handling | 🔴 High | Swallowed exceptions, context loss, error propagation |
| Concurrency Safety | 🔴 High | Race conditions, deadlocks, thread-safe collections |
| Compatibility | 🟠 Medium-High | API compatibility, Schema changes, serialization compatibility |
| Performance | 🟡 Medium | N+1 queries, complexity analysis, index utilization |
| Code Quality | 🔵 Basic | Naming, readability, test coverage |

### Professional Review Reports

- **Four-Layer Problem Analysis**: Phenomenon → Root Cause → Impact Scope → Engineering Solution
- **Compatibility Special Assessment**: Independent section detailing all compatibility risks
- **Performance Quantification**: Time/space complexity with typical data volume impact estimation
- **Action Recommendations**: Sorted by ROI to help team prioritize

## Usage

### Command Format

```bash
# Review local uncommitted changes vs HEAD
svn-review

# Review local changes vs version N
svn-review --base rN

# Review difference between version M and N
svn-review --from rM --to rN

# Limit review scope to specific directory
svn-review --path src/core

# Specify SVN repository URL
svn-review --url https://svn.example.com/repo

# Output English report
svn-review --lang en

# Quick scan mode (P0/P1 only)
svn-review --quick
```

Parameters can be combined:
```bash
svn-review --from r200 --to r210 --path src/core
```

### Fuzzy Trigger

Review is automatically triggered even without explicit commands:

- "帮我看看改了什么" → "Help me review what changed"
- "这次改动有没有问题" → "Any issues with this change?"
- "审查SVN代码" → "Review SVN code"
- "检查本地改动" → "Check local changes"
- "review svn变更" → "review svn changes"

## Installation

### Method 1: Direct Use (Recommended)

Copy the contents of `SKILL.md` into Claude Code's custom Skill configuration.

### Method 2: Plugin Installation

1. Clone this repository locally
2. Configure Claude Code to point to this directory

## Report Example

After review, a structured Markdown report is automatically generated:

```markdown
# SVN Code Review Report

**Review Time**: 2026-04-03 10:30
**Scope**: r53207 → r53472
**Risk Level**: 🟠 Medium
**Issues Found**: P0:0 · P1:2 · P2:3 · P3:5

---

## 📊 Change Overview

| Metric | Value |
|--------|-------|
| Files Changed | 15 |
| Lines Added | +328 |
| Lines Deleted | -156 |

## 🔗 Compatibility Special Assessment

[Independent section detailing compatibility risks]

## 🚨 Issues

[P0/P1/P2/P3 categorized with root cause analysis and fix suggestions]

## 📋 Action Recommendations

[Fix tasks sorted by ROI]
```

Reports are automatically saved to `docs/code-review/`.

## Issue Priority

| Level | Trigger Condition | Requirement |
|-------|-------------------|-------------|
| **P0** | Directly exploitable security vulnerability / data loss / service unavailable | Block merge, must fix |
| **P1** | Crash, leak, or data inconsistency under high concurrency/boundary conditions | Strongly recommend fixing |
| **P2** | Occasional issues in normal use, or significant performance degradation at 1M+ scale | Recommend fixing in near term |
| **P3** | Non-critical code quality/readability issues | Optional optimization |

## Project Structure

```
svn-review/
├── SKILL.md                      # Skill main file (Claude Code configuration)
├── README.md                     # Chinese documentation
├── README_en.md                  # English documentation
├── LICENSE                       # MIT License
├── .gitignore                    # Git ignore configuration
└── references/
    └── review-dimensions.md      # Seven-dimensional detailed review checklist
```

## Technical Details

- **Use Cases**: Enterprise SVN repository code review
- **Supported Languages**: Java, Python, Go, JavaScript/TypeScript, C/C++, etc.
- **Required Tools**: SVN CLI (`svn` command)
- **Output Format**: Markdown structured reports

## Maintenance

This Skill is continuously updated based on project experience. Issues and Pull Requests are welcome.

## License

MIT License - See [LICENSE](LICENSE) file
