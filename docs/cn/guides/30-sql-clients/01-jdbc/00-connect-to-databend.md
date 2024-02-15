---
title: "通过 SQL 客户端连接到 Databend"
sidebar_label: "连接到 Databend"
---

import StepsWrap from '@site/src/components/StepsWrap';
import StepContent from '@site/src/components/Steps/step-content';

在本教程中，我们将指导您通过 Databend JDBC 驱动程序作为用户 `root` 连接到 Databend。

<StepsWrap>
<StepContent number="0" title="开始之前">

- 确保您已准备好本地 Databend 实例以进行测试。详细说明请参见 [Docker 和本地部署](../../10-deploy/03-deploying-local.md)。
- 在本教程中，您将使用 `root` 账户连接到 Databend。在部署时，取消注释 [databend-query.toml](https://github.com/datafuselabs/databend/blob/main/scripts/distribution/configs/databend-query.toml) 配置文件中的以下行以选择此账户：

  ```sql title="databend-query.toml"
  [[query.users]]
  name = "root"
  auth_type = "no_password"
  ```

- 确保您已将 Databend JDBC 驱动程序添加到您的 DBeaver 中。详细说明请参见 [将 Databend JDBC 驱动程序添加到 DBeaver](index.md#adding-databend-jdbc-driver-to-dbeaver)。

</StepContent>
<StepContent number="1" title="创建连接">

1. 在 DBeaver 中，首先在 **Database** > **New Database Connection** 中搜索并选择 `databend`，然后点击 **Next**。

![Alt text](@site/docs/public/img/integration/jdbc-new-driver.png)

2. 如有需要，配置您的连接设置。默认设置将连接到作为用户 `root` 的本地 Databend 实例。

![Alt text](@site/docs/public/img/integration/jdbc-connect.png)

3. 点击 **Test Connection** 以检查连接是否成功。

</StepContent>
</StepsWrap>
