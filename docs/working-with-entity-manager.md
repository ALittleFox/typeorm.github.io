# 什么是实体管理器

使用 `EntityManager` 你可以管理 （插入、更新、删除、加载等）任何实体。

实体管理器就像一个把所有实体存储库收集在一个地方的集合。

你可以通过 `getManager()` 或 `Connection` 获取到实体管理器。

如何使用的例子:
 
```typescript
import {getManager} from "typeorm";
import {User} from "./entity/User";

const entityManager = getManager(); // you can also get it via getConnection().manager
const user = await entityManager.findOne(User, 1);
user.name = "Umed";
await entityManager.save(user);
```
