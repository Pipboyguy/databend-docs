```markdown
---
title: TO_BOOLEAN
---

将值转换为 BOOLEAN 数据类型。

## 语法 {/*syntax*/}

```sql
TO_BOOLEAN( <expr> )
```

## 示例 {/*examples*/}

```sql
SELECT TO_BOOLEAN('true');

┌────────────────────┐
│ to_boolean('true') │
├────────────────────┤
│ true               │
└────────────────────┘
```
```