# 使用查询生成器进行更新

You can create `UPDATE` queries using `QueryBuilder`.
Examples:
             
```typescript
import {getConnection} from "typeorm";

await getConnection()
    .createQueryBuilder()
    .update(User)
    .set({ firstName: "Timber", lastName: "Saw" })
    .where("id = :id", { id: 1 })
    .execute();
```

This is the most efficient way in terms of performance to update entities in your database.
