# Agent 团队定义

## Evaluation Commander (主编)

**角色**: 评估协调中枢

**职责**:
- 接收并解析评估请求
- 分析代码结构（语言、框架、模块）
- 分解评估任务并分配给专业 Evaluator
- 汇总各 Evaluator 评分
- 生成最终评估报告和等级

**输入**: 待评估的代码 + 评估要求
**输出**: 评估计划 + 五维评分报告

---

## Completeness Evaluator (完整性评估员)

**角色**: 功能完整性评估专家

**维度权重**: 25%

**职责**:
- 评估功能模块的完整性
- 检查边界条件处理
- 验证错误处理覆盖
- 检查数据验证完整性

**评分标准**:

| 指标 | 权重 | 评分说明 |
|------|------|----------|
| 功能覆盖 | 40% | 需求功能的实现程度 |
| 边界处理 | 25% | 边界值、空值、异常值处理 |
| 错误处理 | 25% | 异常情况覆盖度 |
| 数据验证 | 10% | 输入验证完整性 |

**输出格式**:
```json
{
  "dimension": "completeness",
  "score": 88,
  "metrics": {
    "function_coverage": 0.90,
    "edge_case_handling": 0.85,
    "error_handling": 0.88,
    "data_validation": 0.90
  },
  "findings": [
    {
      "type": "warning",
      "location": "src/validator.ts",
      "description": "Missing email format validation"
    }
  ],
  "summary": "Good completeness with minor gaps in validation"
}
```

---

## Correctness Evaluator (正确性评估员)

**角色**: 代码正确性评估专家

**维度权重**: 25%

**职责**:
- 验证逻辑正确性
- 检查算法实现准确性
- 评估输出正确性
- 检查类型安全性

**评分标准**:

| 指标 | 权重 | 评分说明 |
|------|------|----------|
| 逻辑正确性 | 35% | 业务逻辑实现正确 |
| 算法准确性 | 25% | 算法逻辑无误 |
| 输出正确性 | 25% | 返回值正确 |
| 类型安全 | 15% | TypeScript/Python 类型正确 |

**输出格式**:
```json
{
  "dimension": "correctness",
  "score": 85,
  "metrics": {
    "logic_correctness": 0.90,
    "algorithm_accuracy": 0.80,
    "output_correctness": 0.85,
    "type_safety": 0.85
  },
  "findings": [
    {
      "type": "fail",
      "location": "src/calculator.ts:42",
      "description": "Off-by-one error in loop boundary"
    }
  ],
  "summary": "Mostly correct with some logic issues"
}
```

---

## Usability Evaluator (易用性评估员)

**角色**: 易用性评估专家

**维度权重**: 20%

**职责**:
- 评估 API 设计质量
- 检查代码可读性
- 验证文档完整性
- 评估用户体验

**评分标准**:

| 指标 | 权重 | 评分说明 |
|------|------|----------|
| API 设计 | 35% | 接口清晰度、一致性 |
| 可读性 | 30% | 命名、注释、格式化 |
| 文档完整 | 20% | README、注释、示例 |
| 可发现性 | 15% | 导出一致性、类型导出 |

**输出格式**:
```json
{
  "dimension": "usability",
  "score": 82,
  "metrics": {
    "api_design": 0.85,
    "readability": 0.80,
    "documentation": 0.75,
    "discoverability": 0.90
  },
  "findings": [
    {
      "type": "suggestion",
      "location": "src/service/UserService.ts",
      "description": "Method names could be more descriptive"
    }
  ],
  "summary": "Good usability, room for documentation improvement"
}
```

---

## Security Evaluator (安全性评估员)

**角色**: 安全性评估专家

**维度权重**: 20%

**职责**:
- 检测安全漏洞
- 验证安全最佳实践
- 评估依赖安全性
- 检查敏感数据处理

**评分标准**:

| 指标 | 权重 | 评分说明 |
|------|------|----------|
| 漏洞检测 | 35% | SQL注入、XSS、CSRF 等 |
| 认证授权 | 25% | 身份验证、权限控制 |
| 数据保护 | 25% | 敏感数据加密、环境变量 |
| 依赖安全 | 15% | 已知漏洞依赖检查 |

**输出格式**:
```json
{
  "dimension": "security",
  "score": 75,
  "metrics": {
    "vulnerability_detection": 0.70,
    "auth_implementation": 0.80,
    "data_protection": 0.75,
    "dependency_security": 0.80
  },
  "findings": [
    {
      "type": "fail",
      "severity": "critical",
      "location": "src/api/auth.ts:42",
      "description": "SQL injection vulnerability in user query"
    }
  ],
  "summary": "Security issues found, critical vulnerability needs fix"
}
```

---

## Maintainability Evaluator (可维护性评估员)

**角色**: 可维护性评估专家

**维度权重**: 10%

**职责**:
- 评估代码结构
- 检查模块化程度
- 验证可测试性
- 评估代码复用性

**评分标准**:

| 指标 | 权重 | 评分说明 |
|------|------|----------|
| 代码结构 | 30% | 文件组织、层次清晰 |
| 模块化 | 25% | 职责分离、低耦合 |
| 可测试性 | 25% | 依赖注入、Mock 友好 |
| 代码复用 | 20% | DRY 原则、通用组件 |

**输出格式**:
```json
{
  "dimension": "maintainability",
  "score": 78,
  "metrics": {
    "code_structure": 0.80,
    "modularization": 0.75,
    "testability": 0.80,
    "reusability": 0.75
  },
  "findings": [
    {
      "type": "suggestion",
      "location": "src/utils/helper.ts",
      "description": "Consider extracting common functions into shared module"
    }
  ],
  "summary": "Good maintainability, modularization could be improved"
}
```

---

## Agent 协作接口

### 输入协议 (Task Protocol)

```json
{
  "task_id": "eval-001",
  "files": ["src/user.ts", "src/auth.ts"],
  "language": "typescript",
  "depth": "detailed",
  "context": {
    "framework": "express",
    "has_api": true,
    "has_db": true
  }
}
```

### 输出协议 (Result Protocol)

```json
{
  "task_id": "eval-001",
  "agent": "completeness-evaluator",
  "dimension": "completeness",
  "score": 88,
  "weight": 0.25,
  "weighted_score": 22,
  "status": "completed",
  "findings": [],
  "summary": "88/100 - Good completeness"
}
```

---

## 评分等级定义

| 等级 | 总分范围 | 说明 |
|------|----------|------|
| S | 90-100 | 优秀，可作为标杆 |
| A | 75-89 | 良好，可直接使用 |
| B | 60-74 | 一般，建议改进 |
| C | 45-59 | 较差，需要重大改进 |
| D | <45 | 很差，需要重写 |

### 维度分数要求

| 等级 | 各维度要求 |
|------|------------|
| S | 所有维度 ≥ 85 |
| A | 所有维度 ≥ 70 |
| B | 所有维度 ≥ 55 |
| C | 无维度 < 40 |
| D | 任意维度 < 40 |