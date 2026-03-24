# Cache Development Skill

一个用于 InterSystems Cache 数据库开发的 AI Agent Skill，包含完整的 ObjectScript 和 SQL 参考文档。

## 功能特性

- **完整的 ObjectScript 参考**: 包含所有命令、函数、特殊变量的详细文档
- **完整的 SQL 参考**: 包含所有 SQL 命令、函数、谓词的详细文档
- **速查表**: 常用语法和模式的快速参考
- **Cache vs 标准 SQL 差异**: 帮助理解 Cache SQL 的特有语法
- **常见模式**: 数据访问、类定义、CRUD 操作等常见代码模式

## 文档内容

| 分类 | 文件数 | 内容 |
|------|--------|------|
| ObjectScript 命令 | 39 | SET, IF, FOR, DO, KILL, LOCK 等 |
| ObjectScript 函数 | 76 | $GET, $PIECE, $LIST, $EXTRACT 等 |
| ObjectScript 数学/时间函数 | 22 | $ZDATE, $ZDATETIME 等 |
| ObjectScript 特殊变量 | 45 | $HOROLOG, $IO, $JOB 等 |
| ObjectScript 系统函数 | 25 | $ZCONVERT, $ZF 等 |
| SQL 命令 | 62 | SELECT, INSERT, UPDATE, CREATE TABLE 等 |
| SQL 函数 | 100 | CAST, COALESCE, DATEADD 等 |
| SQL 谓词 | 19 | LIKE, IN, BETWEEN, %STARTSWITH 等 |
| SQL 聚合函数 | 10 | AVG, COUNT, SUM, MAX, MIN 等 |

## 安装

### 使用 skills CLI 安装

```bash
npx skills add [your-username]/cache-skill -y
```

### 手动安装

1. 克隆此仓库
2. 将整个目录复制到 AI Agent 的 skills 目录:
   - **Claude Code**: `~/.claude/skills/cache-dev/`
   - **OpenCode**: `.agents/skills/cache-dev/`

## 使用方式

当进行 Cache 数据库开发时，Skill 会自动触发：

- 编写 `.cls` 文件时
- 使用 ObjectScript 语法时
- 编写 Cache SQL 查询时
- 提及 Cache/InterSystems/ObjectScript 相关关键词时

### 文档查阅

所有参考文档位于 `docs/` 目录:

```
docs/
├── objectscript/
│   ├── commands/          # ObjectScript 命令
│   ├── functions/         # ObjectScript 函数
│   ├── math-functions/    # 数学和时间函数
│   ├── special-variables/ # 特殊变量
│   └── system-functions/  # 系统函数
└── sql/
    ├── commands/          # SQL 命令
    ├── functions/         # SQL 函数
    ├── predicates/        # SQL 谓词
    └── aggregate-functions/ # SQL 聚合函数
```

### 参考文档

```
references/
├── quick-reference.md     # 常用语法速查表
├── sql-vs-standard.md     # Cache SQL vs 标准 SQL 差异
└── patterns.md            # 常见代码模式
```

## 示例

### ObjectScript 基础
```objectscript
; 变量操作
SET name = "张三"
SET age = 30

; 全局变量访问
SET ^Person(1, "name") = "张三"
SET name = $GET(^Person(1, "name"), "Unknown")

; 字符串操作
SET str = "apple,banana,cherry"
SET first = $PIECE(str, ",", 1)  ; "apple"
```

### SQL 查询
```sql
-- Cache 特有语法
SELECT TOP 10 * FROM MyApp.Person WHERE Name %STARTSWITH '张'

-- 插入或更新 (Cache 原生支持)
INSERT OR UPDATE INTO MyApp.Person (ID, Name, Age) VALUES (1, '张三', 31)
```

## 许可证

MIT License

## 数据来源

文档来源于 InterSystems Cache 官方文档 (DocBook v2016.2)
