# 评估用例池

> 测试员Agent使用此文档执行完整测试。

---

## 语法验证

| 用例ID | 测试项 | 输入 | 预期结果 | 通过条件 |
|--------|--------|------|----------|----------|
| TC-SYNTAX-001 | frontmatter格式 | SKILL.md内容 | YAML格式正确 | 以`---`开头和结尾 |
| TC-SYNTAX-002 | 必需字段 | frontmatter | 包含name/description/version/author/license/platforms/metadata | 8个字段都存在 |
| TC-SYNTAX-003 | description长度 | description内容 | ≤1024字符 | `len(description) <= 1024` |
| TC-SYNTAX-004 | description开头 | description内容 | 以"Use when"开头 | `description.startswith("Use when")` |
| TC-SYNTAX-005 | version字段 | version内容 | 符合语义化版本 | `x.y.z`格式 |

---

## 功能测试

### 代码评估触发词测试

| 用例ID | 测试项 | 输入 | 预期结果 | 通过条件 |
|--------|--------|------|----------|----------|
| TC-FUNC-001 | 英文触发 | "evaluate code" | 正确识别 | 触发skill |
| TC-FUNC-002 | 中文触发 | "代码评估" | 正确识别 | 触发skill |
| TC-FUNC-003 | 五维触发 | "五维评估" | 正确识别 | 触发skill |
| TC-FUNC-004 | 质量评分触发 | "质量评分" | 正确识别 | 触发skill |

### 五维评估测试

| 用例ID | 测试项 | 输入 | 预期结果 | 通过条件 |
|--------|--------|------|----------|----------|
| TC-FUNC-010 | 完整性评估 | 边界缺失代码 | 完整性维度评分 | 分数合理 |
| TC-FUNC-011 | 正确性评估 | 逻辑错误代码 | 正确性维度评分 | 分数合理 |
| TC-FUNC-012 | 易用性评估 | API设计差代码 | 易用性维度评分 | 分数合理 |
| TC-FUNC-013 | 安全性评估 | SQL注入代码 | 安全性维度低分 | 正确检测 |
| TC-FUNC-014 | 可维护性评估 | 结构混乱代码 | 可维护性维度低分 | 正确检测 |

### 评分等级测试

| 用例ID | 测试项 | 输入 | 预期结果 | 通过条件 |
|--------|--------|------|----------|----------|
| TC-FUNC-020 | S级评分 | 优秀代码 | Grade: S (90+) | 评分正确 |
| TC-FUNC-021 | A级评分 | 良好代码 | Grade: A (75-89) | 评分正确 |
| TC-FUNC-022 | B级评分 | 一般代码 | Grade: B (60-74) | 评分正确 |
| TC-FUNC-023 | C级评分 | 较差代码 | Grade: C (45-59) | 评分正确 |
| TC-FUNC-024 | D级评分 | 很差代码 | Grade: D (<45) | 评分正确 |

---

## 边界测试

| 用例ID | 测试项 | 输入 | 预期结果 | 通过条件 |
|--------|--------|------|----------|----------|
| TC-BOUND-001 | 空输入 | 空字符串 | 返回友好提示 | 不崩溃 |
| TC-BOUND-002 | 超长代码 | >10000行 | 部分评估或超时 | 不崩溃 |
| TC-BOUND-003 | 特殊文件 | 二进制内容 | 跳过或报错 | 不崩溃 |
| TC-BOUND-004 | 未知语言 | 自定义语言 | 使用通用规则 | 不崩溃 |

---

## 安全测试

| 用例ID | 测试项 | 输入 | 预期结果 | 通过条件 |
|--------|--------|------|----------|----------|
| TC-SEC-001 | 敏感信息 | 密码/token在代码中 | 不出现在输出 | 无泄露 |
| TC-SEC-002 | 注入漏洞评估 | SQL/XSS注入代码 | 安全性维度低分 | 正确评估 |
| TC-SEC-003 | 越权风险评估 | 缺少权限检查代码 | 安全性维度扣分 | 正确评估 |

---

## 输出格式测试

| 用例ID | 测试项 | 输入 | 预期结果 | 通过条件 |
|--------|--------|------|----------|----------|
| TC-OUT-001 | Markdown格式 | 评估结果 | 符合模板格式 | 格式正确 |
| TC-OUT-002 | HTML格式 | `--format html` | 完整CSS和HTML | 可渲染 |
| TC-OUT-003 | 五维雷达图数据 | 评估结果 | 包含各维度分数 | 数据准确 |
| TC-OUT-004 | 加权总分计算 | 多维度评分 | 总分=加权求和 | 计算正确 |

---

## 完整代码评估用例

### EC-001: TypeScript 函数评估

**输入代码**:
```typescript
export function calculateTotal(items: { price: number; quantity: number }[]): number {
  let total = 0;
  for (const item of items) {
    total += item.price * item.quantity;
  }
  return total;
}

export function validateEmail(email: string): boolean {
  return email.includes('@');
}
```

**预期五维评分**:
| 维度 | 预期分数 | 说明 |
|------|----------|------|
| 完整性 | 70 | 边界处理缺失（空数组） |
| 正确性 | 85 | 逻辑基本正确 |
| 易用性 | 75 | API 清晰但缺少文档 |
| 安全性 | 90 | 无安全风险 |
| 可维护性 | 80 | 代码简洁易维护 |
| **总分** | **81** | Grade: A |

---

### EC-002: Python 类评估

**输入代码**:
```python
class UserService:
    def __init__(self, db):
        self.db = db

    def create_user(self, email: str, name: str):
        if '@' not in email:
            raise ValueError('Invalid email')
        self.db.execute(
            "INSERT INTO users (email, name) VALUES (?, ?)",
            (email, name)
        )

    def get_user(self, user_id: int):
        cursor = self.db.execute(
            f"SELECT * FROM users WHERE id = {user_id}"
        )
        return cursor.fetchone()
```

**预期发现**:
- ❌ SQL 注入漏洞（get_user 方法）
- ⚠️ 缺少错误处理（create_user）
- 💡 缺少类型注解

---

### EC-003: Go 函数评估

**输入代码**:
```go
package service

type User struct {
    ID    int
    Email string
    Name  string
}

func GetUser(id int) (*User, error) {
    query := fmt.Sprintf("SELECT * FROM users WHERE id = %d", id)
    row := db.QueryRow(query)

    var user User
    err := row.Scan(&user.ID, &user.Email, &user.Name)
    if err != nil {
        return nil, err
    }
    return &user, nil
}
```

**预期发现**:
- ❌ SQL 注入风险（格式化字符串）
- ⚠️ 缺少 context 传播
- 💡 考虑使用 sqlx 或其他 SQL Builder

---

## 单维度评估用例

### EC-101: 完整性评估 - 边界处理

**输入代码**:
```typescript
function processItems(items: string[]) {
  return items.map(item => item.toUpperCase());
}
```

**评估要点**:
- 空数组处理 ✅ (JavaScript 空数组 map 返回空数组)
- null/undefined 输入 ❌ (会抛出错误)

**预期评分**: 75/100（边界处理不足）

---

### EC-102: 正确性评估 - 逻辑错误

**输入代码**:
```python
def calculate_discount(price: float, discount_percent: float) -> float:
    discount = price * discount_percent
    return price - discount  # 正确

# 测试用例
assert calculate_discount(100, 0.2) == 80  # 正确
```

**评估要点**:
- 逻辑正确性 ✅
- 边界情况（负数折扣）❌

**预期评分**: 80/100

---

### EC-103: 易用性评估 - API 设计

**输入代码**:
```typescript
// 模糊的命名
function d(v: any): any {
  return v;
}

// 不清晰的参数顺序
function createUser(data: any, validate: boolean, notify: boolean) {
  // ...
}
```

**评估要点**:
- 函数命名不清晰 ❌
- 参数顺序不合理 ❌
- 缺少 JSDoc 注释 ❌

**预期评分**: 50/100

---

### EC-104: 安全性评估 - 注入漏洞

**输入代码**:
```typescript
// XSS 漏洞
function renderHTML(userInput: string): string {
  return `<div>${userInput}</div>`;
}

// SQL 注入漏洞
function findUser(userId: string) {
  return db.query(`SELECT * FROM users WHERE id = ${userId}`);
}
```

**评估要点**:
- XSS 漏洞 ❌ (未转义)
- SQL 注入 ❌ (参数拼接)

**预期评分**: 30/100 (Critical issues)

---

### EC-105: 可维护性评估 - 模块化

**输入代码**:
```typescript
// 2000 行的巨大函数
async function processOrder(orderId: string) {
  // 验证订单
  // 查询用户
  // 检查库存
  // 计算价格
  // 创建订单
  // 发送邮件
  // 记录日志
  // ... (总计 2000 行)
}
```

**评估要点**:
- 代码结构混乱 ❌
- 违反单一职责 ❌
- 难以测试 ❌

**预期评分**: 40/100

---

## 综合评估用例

### EC-201: 完整模块评估

**输入**: 一个用户管理模块

```typescript
// user-service.ts
export interface User {
  id: string;
  email: string;
  name: string;
  createdAt: Date;
}

export class UserService {
  constructor(private db: Database) {}

  async createUser(data: { email: string; name: string }): Promise<User> {
    if (!data.email || !data.email.includes('@')) {
      throw new ValidationError('Invalid email');
    }

    const id = generateId();
    const user: User = {
      id,
      email: data.email,
      name: data.name,
      createdAt: new Date(),
    };

    await this.db.insert('users', user);
    return user;
  }

  async getUser(id: string): Promise<User | null> {
    const row = await this.db.query(
      'SELECT * FROM users WHERE id = $1',
      [id]
    );
    return row || null;
  }

  async updateUser(id: string, data: Partial<User>): Promise<User> {
    const user = await this.getUser(id);
    if (!user) throw new NotFoundError('User not found');

    const updated = { ...user, ...data };
    await this.db.update('users', id, updated);
    return updated;
  }

  async deleteUser(id: string): Promise<void> {
    await this.db.delete('users', id);
  }
}
```

**预期五维评分**:
| 维度 | 分数 | 分析 |
|------|------|------|
| 完整性 | 85 | 基本 CRUD 完整，缺少批量操作 |
| 正确性 | 90 | 逻辑正确，错误处理完善 |
| 易用性 | 85 | API 清晰，有类型定义 |
| 安全性 | 88 | 使用参数化查询，无明显漏洞 |
| 可维护性 | 85 | 结构清晰，依赖注入良好 |
| **总分** | **87** | Grade: A |

---

### EC-202: 安全敏感模块评估

**输入**: 认证模块

```typescript
// auth-service.ts
export class AuthService {
  async login(email: string, password: string): Promise<string> {
    const user = await this.db.findUserByEmail(email);
    if (!user) throw new AuthError('Invalid credentials');

    const isValid = await bcrypt.compare(password, user.passwordHash);
    if (!isValid) throw new AuthError('Invalid credentials');

    return this.generateToken(user);
  }

  async register(email: string, password: string): Promise<User> {
    if (password.length < 8) {
      throw new ValidationError('Password too short');
    }

    const passwordHash = await bcrypt.hash(password, 10);
    return this.userService.createUser({ email, passwordHash });
  }
}
```

**预期评分**: 92/100 (S级 - 安全实践良好)

---

### EC-203: 遗留代码评估

**输入**: 老旧代码库片段

```javascript
// legacy.js - 2009 年写的代码
function calc(x, y) {
  var result = x + y;
  if (x > 100)
  result = x - y;
  if (y > 50)
  result = result * 2;
  return result;
}
```

**预期评分**: 45/100 (D级)

**问题**:
- 变量命名不清晰 (x, y, result)
- 缺少大括号容易出错
- 无类型注解
- 无测试
- 无文档