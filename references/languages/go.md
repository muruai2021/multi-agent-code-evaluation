# Go 代码评估要点

## 评估框架

### 完整性 (Completeness)

**检查项**:
- [ ] 函数文档完整（godoc 格式）
- [ ] 错误类型定义
- [ ] 接口定义清晰
- [ ] context 传播

**评分标准**:
| 指标 | 描述 |
|------|------|
| 文档覆盖率 | 有文档的导出函数 / 总导出函数 |
| 错误处理覆盖 | 主要函数有错误处理 |
| 接口定义 | 关键类型有接口实现 |

### 正确性 (Correctness)

**检查项**:
- [ ] `go vet` 无警告
- [ ] 错误处理完整（不忽略 err）
- [ ] 并发安全（goroutine + channel）
- [ ] 资源正确释放（defer）

**常见错误**:
```go
// ❌ 错误示例
func getUser(id int) *User {
    user, _ := db.Query(id)  // 忽略错误
    return user
}

go func() {
    doSomething()
}()  // 不等待完成

// ✅ 正确示例
func getUser(ctx context.Context, id int) (*User, error) {
    user, err := db.QueryContext(ctx, id)
    if err != nil {
        return nil, fmt.Errorf("query user: %w", err)
    }
    return user, nil
}

var wg sync.WaitGroup
wg.Add(1)
go func() {
    defer wg.Done()
    doSomething()
}()
wg.Wait()
```

### 易用性 (Usability)

**检查项**:
- [ ] 函数命名遵循惯例
- [ ] 包结构清晰
- [ ] README 完整
- [ ] 示例代码存在

**文档标准**:
```go
// Package user provides user management functionality.
//
// The package supports user registration, authentication, and profile management.
package user

// FindByID retrieves a user by their unique identifier.
//
// It returns the user if found, or nil if the user does not exist.
// The context is used for timeout and cancellation.
func FindByID(ctx context.Context, id int) (*User, error) {
    // implementation
}
```

### 安全性 (Security)

**检查项**:
- [ ] 无 SQL 注入风险
- [ ] 敏感数据不日志
- [ ] 认证/授权正确
- [ ] 依赖无已知漏洞（go audit）

**安全模式**:
```go
// ✅ 使用参数化查询
func findUser(ctx context.Context, db *sql.DB, id string) (*User, error) {
    row := db.QueryRowContext(ctx,
        "SELECT id, name FROM users WHERE id = $1", id)
    // ...
}

// ❌ 不日志敏感信息
logger.Printf("User logged in: %s", email)  // OK
logger.Printf("Password: %s", password)     // NEVER
```

### 可维护性 (Maintainability)

**检查项**:
- [ ] 格式化（gofmt）
- [ ] 圈复杂度 < 10
- [ ] 无重复代码
- [ ] 测试覆盖 > 60%

**代码组织**:
```
src/
├── user/           # user 包
│   ├── user.go    # 用户实体和接口
│   ├── service.go # 业务逻辑
│   ├── repo.go    # 数据访问
│   └──_test.go    # 测试
├── auth/          # 认证包
└── errors.go      # 错误定义
```

---

## 框架特定评估

### Gin (HTTP 框架)

**安全性**:
- [ ] 中间件正确使用
- [ ] CORS 配置正确
- [ ] 限流配置

**正确性**:
- [ ] 路由参数校验
- [ ] 绑定验证（validator）

### gRPC

**完整性**:
- [ ] proto 文件定义完整
- [ ] 错误处理规范

**正确性**:
- [ ] 流处理正确
- [ ] 超时设置

---

## 评分计算

### Go 专项评分

```go
type GoScore struct {
    Completeness struct {
        Documentation float64 // 文档覆盖率
        ErrorHandling float64 // 错误处理覆盖
        InterfaceDef  float64 // 接口定义完整度
    }
    Correctness struct {
        VetCheck     int     // go vet 错误数
        ErrorHandle  float64 // 错误处理完整度
        Concurrency  float64 // 并发安全度
    }
    Usability struct {
        Naming      float64 // 命名规范度
        PackageStr  float64 // 包结构清晰度
        Documentation float64 // 文档完整性
    }
    Security struct {
        SQLInjection bool    // SQL 注入风险
        SecretHandle float64 // 敏感信息处理
    }
    Maintainability struct {
        Formatting   float64 // 格式化
        Complexity   float64 // 圈复杂度
        TestCoverage float64 // 测试覆盖率
    }
}
```

### 权重调整

| 维度 | 权重 | Go 特有调整 |
|------|------|-------------|
| 完整性 | 25% | +5% 错误处理 |
| 正确性 | 25% | +5% go vet 检查 |
| 易用性 | 20% | - |
| 安全性 | 20% | +5% SQL 注入防护 |
| 可维护性 | 10% | +5% 依赖管理 |

---

## Go 惯用模式检查

### 错误处理（必须）
```go
// ✅ 正确模式
if err != nil {
    return fmt.Errorf("operation failed: %w", err)
}

// ❌ 错误模式
if err != nil {
    return err  // 丢失上下文
}
// 或者
_ = err  // 忽略错误
```

### Context 传播（必须）
```go
// ✅ 正确
func doSomething(ctx context.Context) error {
    return callExternal(ctx)
}

// ❌ 错误
func doSomething() error {
    return callExternal()  // 丢失 context
}
```

### 并发安全
```go
// ✅ 使用 sync 包或 channel
var mu sync.Mutex
mu.Lock()
defer mu.Unlock()

// 或使用 channel
ch := make(chan Result)
```