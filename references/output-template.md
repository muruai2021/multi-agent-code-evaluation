# 评估报告模板

## Markdown 格式

```markdown
# Code Evaluation Report

## Summary
- **Overall Score**: 82/100 (Grade: A)
- **Files Evaluated**: 8
- **Language**: TypeScript
- **Date**: 2024-01-01

## Five-Dimensional Assessment

| Dimension | Weight | Score | Weighted |
|-----------|--------|-------|----------|
| Completeness | 25% | 88 | 22.0 |
| Correctness | 25% | 85 | 21.25 |
| Usability | 20% | 82 | 16.4 |
| Security | 20% | 75 | 15.0 |
| Maintainability | 10% | 78 | 7.8 |
| **Total** | 100% | - | **82.45** |

🎯 [grade] **A (82/100)** - 良好，可直接使用

## Dimension Breakdown

### Completeness (完整性) - 88/100

💯 [score] 88/100 ⚖️ [weight] 25%

📊 [metric] Function coverage: 90%
📊 [metric] Edge case handling: 85%
📊 [metric] Error handling: 88%

✅ [pass] 完整的类型注解
✅ [pass] 良好的错误处理
⚠️ [warning] 缺少批量操作支持

### Correctness (正确性) - 85/100

💯 [score] 85/100 ⚖️ [weight] 25%

📊 [metric] Logic correctness: 90%
📊 [metric] Type safety: 85%
📊 [metric] Output correctness: 80%

✅ [pass] 业务逻辑正确
✅ [pass] 异步处理正确
⚠️ [warning] 部分边界情况可优化

### Security (安全性) - 75/100

💯 [score] 75/100 ⚖️ [weight] 20%

📊 [metric] Vulnerability detection: 70%
📊 [metric] Auth implementation: 80%
📊 [metric] Data protection: 75%

✅ [pass] 密码使用 bcrypt 加密
✅ [pass] 使用参数化查询
❌ [fail] 缺少 CSRF 防护

## Findings

### ❌ [fail] Critical Issues (2)

1. **SQL 注入风险**
   - Location: `src/db.ts:42`
   - Description: 使用字符串拼接构建 SQL
   - Suggestion: 使用参数化查询

2. **缺少输入验证**
   - Location: `src/api/user.ts:15`
   - Description: 用户名无长度限制
   - Suggestion: 添加验证规则

### ⚠️ [warning] Warnings (3)

- 函数复杂度较高 (max: 12)
- 缺少 API 文档
- 部分变量命名不清晰

### 💡 [suggestion] Suggestions (5)

- 考虑添加批量操作支持
- 建议增加单元测试覆盖
- 可以提取通用工具函数

## Recommendations

### High Priority
1. 修复 SQL 注入漏洞
2. 添加输入验证

### Medium Priority
3. 增加单元测试覆盖
4. 补充 API 文档

### Low Priority
5. 优化函数命名
6. 提取通用组件
```

---

## HTML 格式

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Code Evaluation Report</title>
    <style>
        :root {
            --bg-primary: #ffffff;
            --bg-secondary: #f6f8fa;
            --bg-tertiary: #f0f0f0;
            --text-primary: #24292f;
            --text-secondary: #57606a;
            --border-color: #d0d7de;
            --score-s: #1f883d;
            --score-a: #1a7f37;
            --score-b: #9a6700;
            --score-c: #bf8700;
            --score-d: #d1242f;
            --pass: #1a7f37;
            --fail: #d1242f;
            --warning: #bf8700;
            --suggestion: #8250df;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;
            line-height: 1.6;
            color: var(--text-primary);
            background: var(--bg-primary);
            padding: 2rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .header {
            border-bottom: 1px solid var(--border-color);
            padding-bottom: 1rem;
            margin-bottom: 2rem;
        }

        .title { font-size: 1.75rem; font-weight: 600; margin-bottom: 0.5rem; }

        .meta { color: var(--text-secondary); font-size: 0.875rem; }

        .overall-score {
            display: flex;
            align-items: center;
            gap: 1.5rem;
            padding: 1.5rem;
            background: var(--bg-secondary);
            border-radius: 12px;
            margin-bottom: 2rem;
        }

        .score-circle {
            width: 100px;
            height: 100px;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-weight: 600;
        }

        .score-circle.grade-a { background: #dafbe1; color: var(--score-a); }
        .score-circle.grade-b { background: #fff8c5; color: var(--score-b); }
        .score-circle.grade-c { background: #fff8c5; color: var(--score-c); }
        .score-circle.grade-d { background: #ffeef0; color: var(--score-d); }

        .score-number { font-size: 2rem; }
        .score-label { font-size: 0.75rem; }

        .grade-info h2 { font-size: 1.5rem; margin-bottom: 0.5rem; }
        .grade-info p { color: var(--text-secondary); }

        .dimensions {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1rem;
            margin-bottom: 2rem;
        }

        .dimension-card {
            background: var(--bg-secondary);
            border-radius: 8px;
            padding: 1rem;
            border: 1px solid var(--border-color);
        }

        .dimension-name {
            font-size: 0.875rem;
            color: var(--text-secondary);
            margin-bottom: 0.5rem;
        }

        .dimension-score {
            display: flex;
            align-items: baseline;
            gap: 0.5rem;
        }

        .dimension-score .value {
            font-size: 1.75rem;
            font-weight: 600;
        }

        .dimension-score .max { color: var(--text-secondary); }

        .dimension-bar {
            height: 6px;
            background: var(--bg-tertiary);
            border-radius: 3px;
            margin-top: 0.5rem;
            overflow: hidden;
        }

        .dimension-bar-fill {
            height: 100%;
            border-radius: 3px;
            background: var(--score-a);
            transition: width 0.3s;
        }

        .dimension-bar-fill.high { background: var(--score-s); }
        .dimension-bar-fill.medium { background: var(--score-b); }
        .dimension-bar-fill.low { background: var(--score-d); }

        .section {
            border: 1px solid var(--border-color);
            border-radius: 6px;
            margin-bottom: 1.5rem;
            overflow: hidden;
        }

        .section-header {
            background: var(--bg-secondary);
            padding: 0.75rem 1rem;
            font-weight: 600;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .section-body { padding: 1rem; }

        .finding-item {
            padding: 1rem;
            border-bottom: 1px solid var(--border-color);
        }

        .finding-item:last-child { border-bottom: none; }

        .finding-header {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            margin-bottom: 0.5rem;
        }

        .finding-title { font-weight: 500; }
        .finding-location { margin-left: auto; color: var(--text-secondary); font-size: 0.875rem; }

        .finding-tag {
            display: inline-flex;
            padding: 0.125rem 0.5rem;
            border-radius: 12px;
            font-size: 0.75rem;
            font-weight: 500;
        }

        .tag-pass { background: #dafbe1; color: var(--pass); }
        .tag-fail { background: #ffeef0; color: var(--fail); }
        .tag-warning { background: #fff8c5; color: var(--warning); }
        .tag-suggestion { background: #fbefff; color: var(--suggestion); }

        .finding-description { color: var(--text-secondary); margin-left: 1.5rem; }
        .finding-suggestion { margin-top: 0.5rem; margin-left: 1.5rem; padding: 0.5rem; background: var(--bg-secondary); border-radius: 4px; font-size: 0.875rem; }

        .code-block {
            background: var(--bg-tertiary);
            padding: 1rem;
            border-radius: 4px;
            font-family: "SFMono-Regular", Consolas, monospace;
            font-size: 0.875rem;
            overflow-x: auto;
            margin: 0.5rem 0;
        }

        .recommendations { display: grid; grid-template-columns: repeat(3, 1fr); gap: 1rem; }
        .recommendation-level { background: var(--bg-secondary); padding: 1rem; border-radius: 6px; }
        .recommendation-level h4 { margin-bottom: 0.5rem; }
        .recommendation-level.high h4 { color: var(--fail); }
        .recommendation-level.medium h4 { color: var(--warning); }
        .recommendation-level.low h4 { color: var(--pass); }

        .recommendation-level ul { padding-left: 1.25rem; color: var(--text-secondary); font-size: 0.875rem; }

        @media (max-width: 768px) {
            body { padding: 1rem; }
            .dimensions { grid-template-columns: 1fr 1fr; }
            .recommendations { grid-template-columns: 1fr; }
            .overall-score { flex-direction: column; text-align: center; }
        }
    </style>
</head>
<body>
    <div class="header">
        <h1 class="title">Code Evaluation Report</h1>
        <div class="meta">
            <span>Files: 8</span> | <span>Language: TypeScript</span> | <span>Date: 2024-01-01</span>
        </div>
    </div>

    <div class="overall-score">
        <div class="score-circle grade-a">
            <span class="score-number">82</span>
            <span class="score-label">/100</span>
        </div>
        <div class="grade-info">
            <h2>Grade: A</h2>
            <p>良好，可直接使用。各维度评分均衡，无严重问题。</p>
        </div>
    </div>

    <div class="dimensions">
        <div class="dimension-card">
            <div class="dimension-name">Completeness 完整性</div>
            <div class="dimension-score">
                <span class="value">88</span>
                <span class="max">/100</span>
                <span class="dimension-weight">(25%)</span>
            </div>
            <div class="dimension-bar">
                <div class="dimension-bar-fill high" style="width: 88%"></div>
            </div>
        </div>
        <div class="dimension-card">
            <div class="dimension-name">Correctness 正确性</div>
            <div class="dimension-score">
                <span class="value">85</span>
                <span class="max">/100</span>
                <span class="dimension-weight">(25%)</span>
            </div>
            <div class="dimension-bar">
                <div class="dimension-bar-fill high" style="width: 85%"></div>
            </div>
        </div>
        <div class="dimension-card">
            <div class="dimension-name">Usability 易用性</div>
            <div class="dimension-score">
                <span class="value">82</span>
                <span class="max">/100</span>
                <span class="dimension-weight">(20%)</span>
            </div>
            <div class="dimension-bar">
                <div class="dimension-bar-fill" style="width: 82%"></div>
            </div>
        </div>
        <div class="dimension-card">
            <div class="dimension-name">Security 安全性</div>
            <div class="dimension-score">
                <span class="value">75</span>
                <span class="max">/100</span>
                <span class="dimension-weight">(20%)</span>
            </div>
            <div class="dimension-bar">
                <div class="dimension-bar-fill medium" style="width: 75%"></div>
            </div>
        </div>
        <div class="dimension-card">
            <div class="dimension-name">Maintainability 可维护性</div>
            <div class="dimension-score">
                <span class="value">78</span>
                <span class="max">/100</span>
                <span class="dimension-weight">(10%)</span>
            </div>
            <div class="dimension-bar">
                <div class="dimension-bar-fill" style="width: 78%"></div>
            </div>
        </div>
    </div>

    <div class="section">
        <div class="section-header">❌ Critical Issues (2)</div>
        <div class="section-body">
            <div class="finding-item">
                <div class="finding-header">
                    <span class="finding-tag tag-fail">❌ fail</span>
                    <span class="finding-title">SQL 注入风险</span>
                    <span class="finding-location">src/db.ts:42</span>
                </div>
                <div class="finding-description">使用字符串拼接构建 SQL 查询，存在注入风险。</div>
                <div class="finding-suggestion">
                    <strong>建议:</strong> 使用参数化查询替代字符串拼接。
                </div>
            </div>
            <div class="finding-item">
                <div class="finding-header">
                    <span class="finding-tag tag-fail">❌ fail</span>
                    <span class="finding-title">缺少输入验证</span>
                    <span class="finding-location">src/api/user.ts:15</span>
                </div>
                <div class="finding-description">用户名无长度限制，可能导致存储问题。</div>
                <div class="finding-suggestion">
                    <strong>建议:</strong> 添加验证规则，限制用户名长度 3-50 字符。
                </div>
            </div>
        </div>
    </div>

    <div class="section">
        <div class="section-header">⚠️ Warnings (3)</div>
        <div class="section-body">
            <div class="finding-item">
                <div class="finding-header">
                    <span class="finding-tag tag-warning">⚠️ warning</span>
                    <span class="finding-title">函数复杂度较高</span>
                    <span class="finding-location">src/service/order.ts:42</span>
                </div>
                <div class="finding-description">圈复杂度为 12，建议控制在 10 以内。</div>
            </div>
        </div>
    </div>

    <div class="section">
        <div class="section-header">💡 Suggestions</div>
        <div class="section-body">
            <div class="finding-item">
                <div class="finding-header">
                    <span class="finding-tag tag-suggestion">💡 suggestion</span>
                    <span class="finding-title">考虑添加批量操作支持</span>
                </div>
                <div class="finding-description">当前只支持单个操作，批量操作可提升性能。</div>
            </div>
        </div>
    </div>

    <div class="section">
        <div class="section-header">📋 Recommendations</div>
        <div class="section-body">
            <div class="recommendations">
                <div class="recommendation-level high">
                    <h4>🔴 High Priority</h4>
                    <ul>
                        <li>修复 SQL 注入漏洞</li>
                        <li>添加输入验证</li>
                    </ul>
                </div>
                <div class="recommendation-level medium">
                    <h4>🟡 Medium Priority</h4>
                    <ul>
                        <li>增加单元测试覆盖</li>
                        <li>补充 API 文档</li>
                    </ul>
                </div>
                <div class="recommendation-level low">
                    <h4>🟢 Low Priority</h4>
                    <ul>
                        <li>优化函数命名</li>
                        <li>提取通用组件</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</body>
</html>
```

---

## 输出格式选择

| 格式 | 适用场景 | 触发关键词 |
|------|----------|-----------|
| Markdown | 文档、GitHub 评论 | `--markdown` / `-m` |
| HTML | 报告展示、邮件发送 | `--html` / `-h` |

### 使用示例

```
evaluate src/**/*.ts --format html > report.html
evaluate src/**/*.ts --output html > evaluation.html
```