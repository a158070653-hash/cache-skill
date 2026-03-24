---
name: cache-dev
description: InterSystems Cache database development skill. Use when working with .cls files, ObjectScript code, Cache SQL queries, global variables ($PIECE, $LIST, $GET), or when mentions of Cache/InterSystems/ObjectScript/globals appear. Trigger on any Cache-related development task.
---

# Cache Development Skill

## Overview

This skill provides guidance for developing with InterSystems Cache database, including ObjectScript programming, Cache SQL, and class definitions. It includes reference documentation and common patterns.

## When to Use This Skill

Trigger this skill when:

- Working with files ending in `.cls`, `.mac`, `.int`
- Writing or reviewing ObjectScript code
- Writing Cache-specific SQL queries
- User mentions: "Cache", "InterSystems", "ObjectScript", "globals", "$PIECE", "$LIST", "%Persistent"
- User asks about Cache syntax, patterns, or best practices

## Documentation Location

All Cache documentation is stored in:

```
D:/cache-docs/
├── objectscript/
│   ├── commands/      # SET, IF, FOR, DO, KILL, etc.
│   ├── functions/     # $GET, $PIECE, $LIST, $EXTRACT, etc.
│   ├── math-functions/# $ZDATE, $ZDATETIME, etc.
│   ├── special-variables/ # $HOROLOG, $IO, $JOB, etc.
│   └── system-functions/  # $ZCONVERT, $ZF, etc.
└── sql/
    ├── commands/      # SELECT, INSERT, UPDATE, CREATE TABLE, etc.
    ├── functions/     # CAST, COALESCE, DATEADD, etc.
    ├── predicates/    # LIKE, IN, BETWEEN, %STARTSWITH, etc.
    └── aggregate-functions/ # AVG, COUNT, SUM, etc.
```

## Workflow

### Step 1: Identify Task Type

Determine what the user is trying to do:

| Signal | Task Type | Next Step |
|--------|-----------|-----------|
| Writing ObjectScript code | `objectscript` | Go to Step 2 |
| Writing SQL queries | `sql` | Go to Step 2 |
| Creating/modifying .cls files | `class-definition` | Go to Step 2 |
| Debugging errors | `error-handling` | Read `objectscript/commands/try.md` and `objectscript/commands/catch.md` |
| Date/time operations | `datetime` | Read `objectscript/math-functions/` for $ZDATE, $ZDATETIME |
| String manipulation | `string` | Read `objectscript/functions/` for $PIECE, $EXTRACT, $LENGTH |
| List operations | `list` | Read `objectscript/functions/` for $LIST, $LISTBUILD, $LISTGET |

### Step 2: Read Quick Reference

Before diving into detailed docs, always read the quick reference first:

```
read: references/quick-reference.md
```

This gives you common patterns and syntax at a glance.

### Step 3: Read Specific Documentation

Based on the task type, read the relevant documentation:

#### ObjectScript Tasks
```bash
# For commands
read: D:/cache-docs/objectscript/commands/{command-name}.md

# For functions
read: D:/cache-docs/objectscript/functions/{function-name}.md

# For special variables
read: D:/cache-docs/objectscript/special-variables/{variable-name}.md
```

#### SQL Tasks
```bash
# For SQL commands
read: D:/cache-docs/sql/commands/{command-name}.md

# For SQL functions
read: D:/cache-docs/sql/functions/{function-name}.md

# For SQL predicates
read: D:/cache-docs/sql/predicates/{predicate-name}.md
```

#### Class Definition Tasks
```bash
# For class keywords
read: references/patterns.md (Class Definition section)
```

### Step 4: Write Code

After reading the documentation:

1. Follow Cache coding conventions
2. Use the patterns from `references/patterns.md`
3. Check for Cache-specific gotchas in `references/sql-vs-standard.md`

## Quick Reference Commands

### ObjectScript Common Commands
| Command | Purpose | Docs |
|---------|---------|------|
| `SET` | Assign value | `objectscript/commands/set.md` |
| `IF/ELSE` | Conditional | `objectscript/commands/if.md` |
| `FOR` | Loop | `objectscript/commands/for.md` |
| `DO` | Call routine | `objectscript/commands/do.md` |
| `QUIT` | Exit/return | `objectscript/commands/quit.md` |
| `RETURN` | Return from method | `objectscript/commands/return.md` |
| `TRY/CATCH` | Error handling | `objectscript/commands/try.md` |

### ObjectScript Common Functions
| Function | Purpose | Docs |
|----------|---------|------|
| `$GET` | Safe value access | `objectscript/functions/_get.md` |
| `$PIECE` | String by delimiter | `objectscript/functions/_piece.md` |
| `$EXTRACT` | String by position | `objectscript/functions/_extract.md` |
| `$LENGTH` | String length | `objectscript/functions/_length.md` |
| `$LIST` | List element | `objectscript/functions/_list.md` |
| `$LISTBUILD` | Build list | `objectscript/functions/_listbuild.md` |
| `$ORDER` | Iterate subscript | `objectscript/functions/_order.md` |
| `$DATA` | Check variable | `objectscript/functions/_data.md` |

### SQL Common Commands
| Command | Purpose | Docs |
|---------|---------|------|
| `SELECT` | Query data | `sql/commands/select.md` |
| `INSERT` | Add data | `sql/commands/insert.md` |
| `UPDATE` | Modify data | `sql/commands/update.md` |
| `DELETE` | Remove data | `sql/commands/delete.md` |
| `CREATE TABLE` | Create table | `sql/commands/create-table.md` |

### SQL Common Functions
| Function | Purpose | Docs |
|----------|---------|------|
| `$LIST` | List operations | `sql/functions/_list.md` |
| `$EXTRACT` | Substring | `sql/functions/_extract.md` |
| `$PIECE` | Delimiter-based | `sql/functions/_piece.md` |
| `COALESCE` | Null handling | `sql/functions/coalesce.md` |
| `CAST` | Type conversion | `sql/functions/cast.md` |

## Error Handling

When you encounter Cache errors:

1. Read `objectscript/commands/try.md` for TRY/CATCH pattern
2. Read `objectscript/special-variables/_ecode.md` for $ECODE
3. Read `objectscript/special-variables/_etrap.md` for $ETRAP

## Important Notes

- **ObjectScript is case-sensitive** for variable names
- **SQL is case-insensitive** by default in Cache
- **Global variables** start with `^` (e.g., `^Person`)
- **Class parameters** use `..` prefix (e.g., `..%New()`)
- **Method calls** use `.` for instance methods, `##class()` for class methods
