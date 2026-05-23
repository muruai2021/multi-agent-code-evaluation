# Python 代码评估要点

## 评估框架

### 完整性 (Completeness)

**检查项**:
- [ ] 函数类型注解完整（参数和返回值）
- [ ] 类型别名定义
- [ ] 自定义异常类定义
- [ ] dataclass 或 Pydantic 模型

**评分标准**:
| 指标 | 描述 |
|------|------|
| 类型注解覆盖率 | 有类型注解的函数 / 总函数 |
| 模型定义完整性 | 数据模型有完整定义 |
| 异常处理覆盖 | 关键操作有异常处理 |

### 正确性 (Correctness)

**检查项**:
- [ ] mypy 类型检查通过
- [ ] 异步函数定义正确（async/await）
- [ ] 上下文管理器正确使用
- [ ] 装饰器使用规范
- [ ] 可变默认参数检查

**常见错误**:
```python
# ❌ 错误示例
def process(items=[]):  # 可变默认参数
    items.append(1)
    return items

def async_func():
    return fetch_data()  # 缺少 await

# ✅ 正确示例
from typing import Optional, List

def process(items: Optional[List[str]] = None) -> List[str]:
    if items is None:
        items = []
    items.append(1)
    return items

async def async_func() -> Data:
    return await fetch_data()
```

### 易用性 (Usability)

**检查项**:
- [ ] docstring 格式规范（Google/NumPy/Sphinx）
- [ ] 函数命名遵循 PEP 8
- [ ] 模块导出一致性
- [ ] 常量定义合理

**文档标准**:
```python
def calculate_discount(price: float, rate: float) -> float:
    """
    Calculate discounted price.

    Args:
        price: Original price
        rate: Discount rate (0.0 - 1.0)

    Returns:
        Discounted price

    Raises:
        ValueError: If price is negative or rate is out of range
    """
    if price < 0:
        raise ValueError("Price must be non-negative")
    return price * (1 - rate)
```

### 安全性 (Security)

**检查项**:
- [ ] 无字符串格式化 SQL
- [ ] 环境变量使用 os.getenv
- [ ] 敏感数据不打印
- [ ] 依赖无已知漏洞

**安全模式**:
```python
# ✅ 使用参数化查询
cursor.execute("SELECT * FROM users WHERE id = ?", (user_id,))

# ✅ 使用环境变量
api_key = os.getenv("API_KEY")
if not api_key:
    raise ValueError("API_KEY not set")

# ❌ 不打印敏感信息
logger.info(f"User login: {email}")  # OK
logger.debug(f"Password: {password}")  # NEVER
```

### 可维护性 (Maintainability)

**检查项**:
- [ ] 代码格式化（black）
- [ ] 导入排序（isort）
- [ ] 圈复杂度 < 10
- [ ] 无重复代码
- [ ] 测试覆盖 > 70%

**代码组织**:
```
src/
├── models/          # 数据模型
│   ├── user.py     # 用户模型
│   └── order.py    # 订单模型
├── services/       # 业务逻辑
├── repositories/   # 数据访问
└── exceptions.py   # 自定义异常
```

---

## 框架特定评估

### Django

**完整性**:
- [ ] Model 字段类型完整
- [ ] Form/Serializer 定义
- [ ] View 逻辑分离

**安全性**:
- [ ] CSRF 防护
- [ ] SQL 注入防护
- [ ] XSS 防护

### FastAPI

**完整性**:
- [ ] Pydantic 模型定义
- [ ] 响应模型定义
- [ ] 依赖注入类型

**正确性**:
- [ ] 异步路由正确
- [ ] 类型验证自动

### Flask

**完整性**:
- [ ] Blueprint 划分
- [ ] 错误处理装饰器

**安全性**:
- [ ] Session 安全配置
- [ ] CORS 配置正确

---

## 评分计算

### Python 专项评分

```python
python_score = {
    "completeness": {
        "type_annotation": coverage_rate,      # 类型注解覆盖率
        "model_definition": completeness_rate, # 模型定义完整度
        "error_handling": handling_rate,        # 错误处理覆盖
    },
    "correctness": {
        "mypy_check": mypy_errors,              # mypy 检查结果
        "async_correctness": async_check,       # 异步正确性
        "mutable_default": mutable_count,       # 可变默认参数数量
    },
    "usability": {
        "docstring_coverage": doc_rate,         # docstring 覆盖率
        "naming_compliance": pep8_rate,         # 命名规范符合度
    },
    "security": {
        "sql_injection": injection_check,       # SQL 注入检查
        "secret_handling": secret_check,         # 敏感信息处理
    },
    "maintainability": {
        "format_compliance": black_rate,        # 格式化符合度
        "complexity": avg_cyclomatic,            # 平均圈复杂度
        "test_coverage": coverage_rate,         # 测试覆盖率
    }
}
```

### 权重调整

| 维度 | 权重 | Python 特有调整 |
|------|------|-----------------|
| 完整性 | 25% | +5% 类型注解覆盖 |
| 正确性 | 25% | +5% mypy 检查通过 |
| 易用性 | 20% | - |
| 安全性 | 20% | +5% SQL 注入防护 |
| 可维护性 | 10% | +5% 代码格式化 |