# TypeScript 代码评估要点

## 评估框架

### 完整性 (Completeness)

**检查项**:
- [ ] 函数类型注解完整（参数和返回值）
- [ ] 接口/类型定义完整
- [ ] 错误类型定义覆盖
- [ ] 泛型约束明确

**评分标准**:
| 指标 | 描述 |
|------|------|
| 类型注解覆盖率 | 有类型注解的函数 / 总函数 |
| 接口完整性 | 所有数据流有类型定义 |
| 错误处理覆盖 | 关键操作有 try-catch |

### 正确性 (Correctness)

**检查项**:
- [ ] TypeScript 编译无错误
- [ ] 类型收窄正确（guard、assertion）
- [ ] 泛型使用正确
- [ ] 异步处理正确（await、Promise）
- [ ] strict 模式合规

**常见错误**:
```typescript
// ❌ 错误示例
const data: any = response;
function process(x: any): any { return x; }

// ✅ 正确示例
const data: unknown = response;
function process<T>(x: T): T { return x; }
```

### 易用性 (Usability)

**检查项**:
- [ ] 函数命名清晰（动宾结构）
- [ ] 接口命名遵循惯例（IUser / UserProps）
- [ ] JSDoc 注释完整
- [ ] 导出一致性（named export vs default）

**API 设计原则**:
1. **一致性**: 参数顺序一致
2. **清晰性**: 返回值类型明确
3. **最小化**: 参数只取必要项

### 安全性 (Security)

**检查项**:
- [ ] 无 `any` 类型暴露外部数据
- [ ] 输入验证使用类型约束
- [ ] 敏感操作有类型检查
- [ ] 无硬编码敏感信息

**安全模式**:
```typescript
// ✅ 使用 unknown + 类型守卫
function processExternalData(data: unknown): string {
  if (isValidResponse(data)) {
    return data.content;
  }
  throw new Error('Invalid data');
}
```

### 可维护性 (Maintainability)

**检查项**:
- [ ] 避免类型别名滥用
- [ ] 泛型不过度复杂
- [ ] 类型映射使用 utility types
- [ ] 避免类型断言过多

**代码组织**:
```
src/
├── types/           # 类型定义
│   ├── user.ts     # 用户相关类型
│   └── api.ts      # API 相关类型
├── interfaces/     # 接口定义
└── utils/          # 类型工具
```

---

## 框架特定评估

### React/Next.js

**完整性**:
- [ ] Props 类型定义
- [ ] State 类型定义
- [ ] Hook 返回值类型

**正确性**:
- [ ] useEffect 依赖正确
- [ ] 事件处理器类型正确
- [ ] 异步状态更新正确

### NestJS

**完整性**:
- [ ] DTO 类型定义
- [ ] 响应类型定义
- [ ] 异常类型定义

**正确性**:
- [ ] 依赖注入类型正确
- [ ] 模块导入类型安全
- [ ] 守卫/拦截器类型正确

---

## 评分计算

### TypeScript 专项评分

```typescript
const typescriptScore = {
  completeness: {
    typeAnnotation: coverageRate,    // 类型注解覆盖率
    interfaceDefinition: completenessRate, // 接口定义完整度
  },
  correctness: {
    compilerErrors: errorCount,
    strictModeCompliance: complianceRate,
    genericCorrectness: correctnessRate,
  },
  security: {
    anyTypeUsage: anyTypeCount,      // 越少越好
    inputValidation: validationCoverage,
  }
};
```

### 权重调整

| 维度 | 权重 | TypeScript 特有调整 |
|------|------|---------------------|
| 完整性 | 25% | +5% 类型注解覆盖 |
| 正确性 | 25% | +5% strict 模式合规 |
| 易用性 | 20% | - |
| 安全性 | 20% | +5% 类型安全 |
| 可维护性 | 10% | - |