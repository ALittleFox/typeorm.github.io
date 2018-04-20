# 什么是存储库

`Repository` 和 `EntityManager` 类似，但是它的操作权限仅限于一个实体。

你可以通过 `getRepository(Entity)`，`Connection#getRepository`，或 `EntityManager#getRepository` 获取存储库。

例如:
 
```typescript
import {getRepository} from "typeorm";
import {User} from "./entity/User";

const userRepository = getRepository(User); // you can also get it via getConnection().getRepository() or getManager().getRepository()
const user = await userRepository.findOneById(1);
user.name = "Umed";
await userRepository.save(user);
```

有3中类型的实体库:
* `Repository` - 适用于任何实体的常规存储库
* `TreeRepository` - 存储库, 继承 `Repository` 适用于树形实体 
（和通过`@Tree`装饰实体类似）。 
具有特殊的方法来适配树形结构。
* `MongoRepository` - 具有特殊函数仅用来适配MongoDB的存储库。
