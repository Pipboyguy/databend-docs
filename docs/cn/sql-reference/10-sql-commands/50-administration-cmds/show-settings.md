---
title: 显示设置
---

import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.314"/>

Databend 提供了多种系统设置，使您能够控制 Databend 的工作方式。此命令显示可用系统设置的当前值和默认值，以及[设置级别](#setting-levels)。要更新设置，请使用 [SET](set-global.md) 或 [UNSET](unset.md) 命令。

- 有些 Databend 行为不能通过系统设置更改；在使用 Databend 时，您必须考虑到这些行为。例如，
  - Databend 将字符串编码为 UTF-8 字符集。
  - Databend 对数组使用基于 1 的编号约定。
- Databend 在系统表 [system.settings](../../00-sql-reference/20-system-tables/system-settings.md) 中存储系统设置。

## 语法

```sql
SHOW SETTINGS [LIKE '<pattern>' | WHERE <expr>] | [LIMIT <limit>]
```

## 设置级别 {#setting-levels}

每个 Databend 设置都有一个级别，可以是 Global、Default 或 Session。下表说明了每个级别的区别：

| 级别    | 描述                                                                                                                                      |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Global  | 此级别的设置被写入元服务，并影响同一租户中的所有集群。此级别的更改具有全局影响，适用于多个集群共享的整个数据库环境。                      |
| Default | 此级别的设置通过 `databend-query.toml` 配置文件配置。此级别的更改仅影响单个查询实例，并特定于配置文件。此级别为各个查询实例提供默认设置。 |
| Session | 此级别的设置限于单个请求或会话。它们具有最窄的范围，仅适用于进行中的特定会话或请求，提供了一种在每个会话基础上自定义设置的方式。          |

## 示例

```sql
SHOW SETTINGS LIMIT 5;

┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│                     name                    │  value │ default │   range  │  level  │                                                                     description                                                                    │  type  │
├─────────────────────────────────────────────┼────────┼─────────┼──────────┼─────────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┼────────┤
│ acquire_lock_timeout                        │ 15     │ 15      │ None     │ DEFAULT │ Sets the maximum timeout in seconds for acquire a lock.                                                                                            │ UInt64 │
│ aggregate_spilling_bytes_threshold_per_proc │ 0      │ 0       │ None     │ DEFAULT │ Sets the maximum amount of memory in bytes that an aggregator can use before spilling data to storage during query execution.                      │ UInt64 │
│ aggregate_spilling_memory_ratio             │ 0      │ 0       │ [0, 100] │ DEFAULT │ Sets the maximum memory ratio in bytes that an aggregator can use before spilling data to storage during query execution.                          │ UInt64 │
│ auto_compaction_imperfect_blocks_threshold  │ 50     │ 50      │ None     │ DEFAULT │ Threshold for triggering auto compaction. This occurs when the number of imperfect blocks in a snapshot exceeds this value after write operations. │ UInt64 │
│ collation                                   │ utf8   │ utf8    │ ["utf8"] │ DEFAULT │ Sets the character collation. Available values include "utf8".                                                                                     │ String │
└───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
```
