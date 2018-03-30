# 使用查询生成器进行插入

你可以使用 `QueryBuilder` 创建 `INSERT` 语句。
例如:

```typescript
import {getConnection} from "typeorm";

await getConnection()
    .createQueryBuilder()
    .insert()
    .into(User)
    .values([
        { firstName: "Timber", lastName: "Saw" }, 
        { firstName: "Phantom", lastName: "Lancer" }
     ])
    .execute();
```

这是从性能方面考量插入多条数据到数据库中最有效的方法。

你也可以用这种方式进行批量插入。
     