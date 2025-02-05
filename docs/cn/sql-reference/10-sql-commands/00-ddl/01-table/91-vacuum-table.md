---
title: VACUUM TABLE
sidebar_position: 17
---
import FunctionDescription from '@site/src/components/FunctionDescription';

<FunctionDescription description="引入或更新于：v1.2.39"/>

import EEFeature from '@site/src/components/EEFeature';

<EEFeature featureName='VACUUM TABLE'/>

VACUUM TABLE 命令通过永久删除表中的历史数据文件来帮助优化系统性能，释放存储空间。这包括：

- 与表相关的快照，以及它们相关的段和块。

- 孤立文件。在 Databend 中，孤立文件指的是不再与表相关联的快照、段和块。孤立文件可能由各种操作和错误生成，例如在数据备份和恢复期间，它们可能会占用宝贵的磁盘空间，并随着时间的推移降低系统性能。

另见：[VACUUM DROP TABLE](91-vacuum-drop-table.md)

### 语法和示例

```sql
VACUUM TABLE <table_name> [ RETAIN n HOURS ] [DRY RUN]
```

- **RETAIN n HOURS**：此选项确定历史数据文件的保留期限。当指定此选项时，Databend 将比较 *n* 的值和设置 `retention_period` 的值，并将使用较大的值作为保留期限。例如，如果指定的 *n* 值大于默认的 `retention_period`，那么 Databend 将保留 *n* 小时的数据文件，因此超过 *n* 小时的历史数据文件将被删除。

    当未指定此选项时，将应用默认的 `retention_period` 设置，即 12 小时。这意味着任何超过 12 小时的历史数据文件都将被删除。

- **DRY RUN**：当指定此选项时，候选的孤立文件不会被删除，而是返回最多 1000 个候选文件的列表，这些文件如果不使用此选项将会被删除。这在您想要在实际删除任何数据文件之前预览 VACUUM TABLE 命令对表的潜在影响时非常有用。例如：

    ```sql
    VACUUM TABLE t RETAIN 0 HOURS DRY RUN;

    +-----------------------------------------------------+
    | Files                                               |
    +-----------------------------------------------------+
    | 1/8/_sg/932addea38c64393b82cb4b8fb7a2177_v3.bincode |
    | 1/8/_b/b68cbe5fe015474d85a92d5f7d1b5d99_v2.parquet  |
    +-----------------------------------------------------+
    ```

### VACUUM TABLE 与 OPTIMIZE TABLE 的比较

Databend 提供了两个命令用于从表中删除历史数据文件：VACUUM TABLE 和 [OPTIMIZE TABLE](60-optimize-table.md)（带有 PURGE 选项）。尽管这两个命令都能永久删除数据文件，但它们在处理孤立文件方面有所不同：OPTIMIZE TABLE 能够删除孤立的快照，以及相应的段和块。然而，可能存在没有任何关联快照的孤立段和块。在这种情况下，只有 VACUUM TABLE 能帮助清理它们。

VACUUM TABLE 和 OPTIMIZE TABLE 都允许您指定一个期限来确定要删除哪些历史数据文件。然而，OPTIMIZE TABLE 要求您事先从查询中获取快照 ID 或时间戳，而 VACUUM TABLE 允许您直接指定保留数据文件的小时数。VACUUM TABLE 通过 DRY RUN 选项在删除之前提供对历史数据文件的增强控制，该选项允许您在应用命令之前预览要删除的数据文件。这提供了一个安全的删除体验，并帮助您避免意外的数据丢失。


|                                                  	| VACUUM TABLE 	| OPTIMIZE TABLE 	|
|--------------------------------------------------	|--------------	|----------------	|
| 关联的快照（包括段和块）                         	| 是           	| 是             	|
| 孤立的快照（包括段和块）                         	| 是           	| 是             	|
| 仅孤立的段和块                                  	| 是           	| 否             	|
| DRY RUN                                         	| 是           	| 否             	|