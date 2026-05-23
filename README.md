# multi-agent-code-evaluation

[English](#english) | [中文](#中文)

---

## English

### Overview

**multi-agent-code-evaluation** is an intelligent multi-agent collaborative code evaluation system designed for AI coding assistants and development teams. It employs a five-dimensional assessment system to provide comprehensive, objective code quality analysis across completeness, correctness, usability, security, and maintainability.

### Key Features

#### Five-Dimensional Assessment System
- **Completeness (25%)** - Function coverage, edge case handling, error handling
- **Correctness (25%)** - Logic correctness, algorithm accuracy, type safety
- **Usability (20%)** - API design, readability, documentation
- **Security (20%)** - Vulnerability detection, auth/authorization, data protection
- **Maintainability (10%)** - Code structure, modularity, testability

#### Multi-Agent Parallel Evaluation
- **Evaluation Commander** - Task coordination and report aggregation
- **Completeness Evaluator** - Functionality completeness assessment
- **Correctness Evaluator** - Logic and algorithm correctness
- **Usability Evaluator** - API design and code readability
- **Security Evaluator** - Vulnerability and security best practices
- **Maintainability Evaluator** - Code structure and modularity

#### Grading System
| Grade | Score | Description |
|-------|-------|-------------|
| S | 90-100 | Excellent, can serve as benchmark |
| A | 75-89 | Good, ready for production |
| B | 60-74 | Average, improvements recommended |
| C | 45-59 | Below average, needs improvement |
| D | <45 | Poor, needs rewrite |

#### Dual Output Format
- **Markdown**: Documentation, GitHub comments
- **HTML**: Reports, email-friendly

### Supported Languages

| Language | Focus Areas |
|----------|-------------|
| TypeScript | Type safety, async patterns |
| Python | Type annotations, exception handling |
| Go | Error handling, concurrency |

### Usage

**Activate via command:**
```
/evaluate <files or module>
```

**Activate via description:**
```
"evaluate this code"
"代码质量评估"
"评估这个模块的质量"
```

**Output format options:**
```
evaluate src/**/*.ts --format markdown    # Default
evaluate src/**/*.ts --format html        # HTML report
```

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                   Evaluation Commander                        │
│                   (Task Coordination)                          │
└───────────────────────────┬─────────────────────────────────┘
                            │
         ┌──────────────────┼──────────────────┐
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐     ┌────────────┐
  │Completeness│     │Correctness │     │ Usability  │
  │   25%      │     │   25%      │     │   20%      │
  └────────────┘     └────────────┘     └────────────┘
         │                  │                  │
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐
  │  Security  │     │Maintain-   │
  │   20%      │     │ ability 10%│
  └────────────┘     └────────────┘
```

### Project Structure

```
multi-agent-code-evaluation/
├── SKILL.md                      # Main entry point
├── README.md                     # This file
├── test_pool.md                  # Evaluation test cases
└── references/
    ├── agents.md                 # Evaluator definitions
    ├── pipeline.md               # Evaluation workflow
    ├── five-dimensions.md       # Five-dimensional system
    ├── evaluation-tags.md       # Tagging system
    ├── output-template.md       # Report templates
    └── languages/
        ├── typescript.md         # TypeScript evaluation
        ├── python.md             # Python evaluation
        └── go.md                 # Go evaluation
```

---

## 中文

### 概述

**multi-agent-code-evaluation** 是一款智能多智能体协作代码评估系统，专为 AI 编程助手和开发团队设计。它采用五维评估体系，从完整性、正确性、易用性、安全性、可维护性五个维度提供全面、客观的代码质量分析。

### 核心功能

#### 五维评估体系
- **完整性 (25%)** - 功能覆盖、边界处理、错误处理
- **正确性 (25%)** - 逻辑正确、算法准确、类型安全
- **易用性 (20%)** - API 设计、可读性、文档
- **安全性 (20%)** - 漏洞检测、认证授权、数据保护
- **可维护性 (10%)** - 代码结构、模块化、可测试性

#### 多智能体并行评估
- **主编 (Evaluation Commander)** - 任务协调与报告汇总
- **完整性评估员 (Completeness Evaluator)** - 功能完整性评估
- **正确性评估员 (Correctness Evaluator)** - 逻辑和算法正确性
- **易用性评估员 (Usability Evaluator)** - API 设计和代码可读性
- **安全性评估员 (Security Evaluator)** - 漏洞和安全最佳实践
- **可维护性评估员 (Maintainability Evaluator)** - 代码结构和模块化

#### 评分等级
| 等级 | 分数 | 说明 |
|------|------|------|
| S | 90-100 | 优秀，可作为标杆 |
| A | 75-89 | 良好，可直接使用 |
| B | 60-74 | 一般，建议改进 |
| C | 45-59 | 较差，需要改进 |
| D | <45 | 很差，需要重写 |

#### 双输出格式
- **Markdown**: 文档、GitHub 评论
- **HTML**: 报告展示、邮件友好

### 支持的语言

| 语言 | 评估重点 |
|------|----------|
| TypeScript | 类型安全、异步模式 |
| Python | 类型注解、异常处理 |
| Go | 错误处理、并发安全 |

### 使用方式

**通过命令激活:**
```
/evaluate <文件或模块>
```

**通过描述激活:**
```
"evaluate this code"
"代码质量评估"
"评估这个模块的质量"
```

**输出格式选项:**
```
evaluate src/**/*.ts --format markdown    # 默认 Markdown
evaluate src/**/*.ts --format html        # HTML 报告
```

### 系统架构

```
┌─────────────────────────────────────────────────────────────┐
│                   评估主编 (Evaluation Commander)            │
│                      (任务协调)                              │
└───────────────────────────┬─────────────────────────────────┘
                            │
         ┌──────────────────┼──────────────────┐
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐     ┌────────────┐
  │   完整性   │     │   正确性   │     │   易用性   │
  │   25%      │     │   25%      │     │   20%      │
  └────────────┘     └────────────┘     └────────────┘
         │                  │                  │
         ▼                  ▼                  ▼
  ┌────────────┐     ┌────────────┐
  │   安全性   │     │  可维护性  │
  │   20%      │     │   10%      │
  └────────────┘     └────────────┘
```

### 项目结构

```
multi-agent-code-evaluation/
├── SKILL.md                      # 主入口
├── README.md                     # 项目文档
├── test_pool.md                  # 评估用例池
└── references/
    ├── agents.md                 # 评估智能体定义
    ├── pipeline.md                # 评估流程
    ├── five-dimensions.md        # 五维评估体系
    ├── evaluation-tags.md        # 标签系统
    ├── output-template.md       # 报告模板
    └── languages/
        ├── typescript.md         # TypeScript 评估要点
        ├── python.md             # Python 评估要点
        └── go.md                 # Go 评估要点
```

---

## License

MIT