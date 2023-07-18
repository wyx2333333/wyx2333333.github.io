---
title: Node.js中实现PostgreSQL触发器事件的监听
date: 2023-2-27 14:25:48 +0800
categories: [Web, Node.js]
tags: [Node.js, PostgreSQL]
---

## PostgreSQL 中配置触发器

### 创建全局触发器函数

- `test_notify`: 触发器函数名称

```sql
CREATE OR REPLACE FUNCTION "public"."test_notify"()
  RETURNS "pg_catalog"."trigger" AS $BODY$
BEGIN
  PERFORM pg_notify('test_notify', json_build_object('operation', TG_OP, 'tableName', TG_TABLE_NAME, 'oldData', row_to_json(OLD), 'newData', row_to_json(NEW))::TEXT);
  RETURN NEW;
END;
$BODY$
  LANGUAGE plpgsql VOLATILE
  COST 100;
```

### table 绑定触发器函数

- `test_notify_trigger`: 触发器名称
- `test_table`: 绑定触发器的表名
- `test_notify`: 前面创建的触发器函数名称

```sql
DROP TRIGGER "test_notify_trigger" ON "public"."test_table";
CREATE TRIGGER "test_notify_trigger" AFTER INSERT ON "public"."test_table"
FOR EACH ROW
EXECUTE PROCEDURE "public"."test_notify"();
```

## 使用 pg 实现监听

### 安装 pg 库

```bash
npm i pg
# or
yarn add pg
# or
pnpm i pg
```

### 事件监听

```typescript
import pg from "pg";

type Payload<T> = {
  operation: "INSERT" | "DELETE" | "UPDATE"; // 数据触发操作类型
  tableName: string; // 触发的表名
  oldData: T;
  newData: T;
};

function listenToNotify() {
  const connectionString =
    "postgres://<user>:<password>@<host>:<port>/<database>?<query>";

  const client = new pg.Client({ connectionString });

  client.on("notification", ({ channel, payload }) => {
    const data: Payload<{}> = JSON.parse(payload || "{}");

    // channel: 触发器函数名称
    if ("test_notify" === channel) {
      // ...
    }
  });
}
```
{: file='listenToNotify.ts'}

> Oracle、MySQL、SQL Server 等数据库都有类似 trigger 实现
{: .prompt-tip }
