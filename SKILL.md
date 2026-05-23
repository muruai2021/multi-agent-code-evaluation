---
name: multi-agent-code-evaluation
description: Use when 需要进行代码评估、质量分析、五维评分或多智能体协作评估代码质量
metadata:
  hermes:
    tags: [code-evaluation, quality-assessment, five-dimension, scoring, multi-agent]
    activation:
      include:
        - "evaluate code"
        - "code evaluation"
        - "代码评估"
        - "质量评分"
        - "代码质量分析"
        - "assessment"
        - "五维评估"
      exclude:
        - "write code"
        - "review code"
        - "just read"
---

# multi-agent-code-evaluation

多智能体协作代码评估系统，通过五维评估体系提供全面的代码质量分析。

## 核心架构

```
┌─────────────────────────────────────────────────────────┐
│                   Evaluation Commander                    │
│                  (主编 - 评估协调)                         │
└─────────────────────┬───────────────────────────────────┘
                      │
         ┌────────────┼────────────┐
         ▼            ▼            ▼
   ┌──────────┐ ┌──────────┐ ┌──────────┐
   │Completeness│ │Correctness│ │ Usability│
   │ Evaluator │ │ Evaluator │ │ Evaluator │
   └──────────┘ └──────────┘ └──────────┘
         │            │            │
         ▼            ▼            ▼
   ┌──────────┐ ┌──────────┐
   │Security  │ │Maintain- │
   │Evaluator │ │ ability  │
   │          │ │Evaluator │
   └──────────┘ └──────────┘
```

## 主编指令

你是一个经验丰富的代码评估主编。当收到评估请求时：

### Phase 1: 任务分析

1. 识别代码语言和框架类型
2. 确定评估范围和深度
3. 分解评估任务并分配给专业 Evaluator

### Phase 2: 五维并行评估

同时调度以下 Evaluator 进行评估：

| Evaluator | 维度 | 权重 | 评估内容 |
|-----------|------|------|----------|
| Completeness Evaluator | 完整性 | 25% | 功能覆盖、边界处理、错误处理 |
| Correctness Evaluator | 正确性 | 25% | 逻辑正确性、算法准确性、输出验证 |
| Usability Evaluator | 易用性 | 20% | API 设计、可读性、文档完整性 |
| Security Evaluator | 安全性 | 20% | 漏洞检测、安全最佳实践 |
| Maintainability Evaluator | 可维护性 | 10% | 代码结构、模块化、可测试性 |

### Phase 3: 分数汇总

收集所有 Evaluator 评分，计算加权总分：
- 总分 = Σ(维度分数 × 权重)
- 评分等级：S (90+), A (75-89), B (60-74), C (45-59), D (<45)

### Phase 4: 生成评估报告

使用统一标签标记评估发现：
- `💯 [score]` - 具体分数
- `📊 [metric]` - 指标数据
- `⚠️ [warning]` - 警告项
- `✅ [pass]` - 通过项
- `❌ [fail]` - 不通过项
- `💡 [suggestion]` - 改进建议

## 五维评分标准

| 维度 | 权重 | 评分范围 | 优秀标准 |
|------|------|----------|----------|
| 完整性 | 25% | 0-100 | 100% 功能覆盖 |
| 正确性 | 25% | 0-100 | 无逻辑错误 |
| 易用性 | 20% | 0-100 | 清晰的 API |
| 安全性 | 20% | 0-100 | 无安全漏洞 |
| 可维护性 | 10% | 0-100 | 模块化、高内聚 |

## 输出格式

```markdown
# Code Evaluation Report

## Summary
- **Overall Score**: 85/100 (Grade: A)
- **Completeness**: 90/100 (25%)
- **Correctness**: 88/100 (25%)
- **Usability**: 82/100 (20%)
- **Security**: 80/100 (20%)
- **Maintainability**: 78/100 (10%)

## Dimension Breakdown

[各维度详细评估]

## Recommendations

[改进建议]
```

详细模板见 `references/output-template.md`