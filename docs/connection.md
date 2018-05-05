# 使用连接

* [什么是`Connection`](#what-is-connection)
* [创建一个新连接](#creating-a-new-connection)
* [使用 `ConnectionManager`](#using-connectionmanager)
* [使用连接](#working-with-connection-1)

## 什么是 `Connection`

只有当你建立了连接你才能和数据库进行交互。

TypeORM的 `Connection` 并不像看起来的那样建立一个数据库连接，而是建立了一个数据库的连接池。

如果你对真正的数据库连接感兴趣，那么请参考`QueryRunner`的文档。

每个 `QueryRunner` 的实例都是一个独立的数据库连接。

连接池的建立是在 `Connection` 调用 `connect` 方法的时候触发的。

通过 `createConnection` 函数建立连接将会自动调用 `connect` 方法。

断开（或关闭连接池中的所有连接）是在调用`close`的时候触发的。

通常来说，你必须在你的应用引导程序中创建一次连接，并在你完全用完数据库后关闭它。

在实际应用中，如果你正在为站点构建后端服务并且后端服务器始终保持运行 - 你永远不要关闭连接。

## 创建一个新连接

有好几种方法可以创建连接。

最简单常用的方法是使用 `createConnection` 和 `createConnections` 函数。

* `createConnection` 创建一个连接：

```typescript
import {createConnection, Connection} from "typeorm";

const connection = await createConnection({
    type: "mysql",
    host: "localhost",
    port: 3306,
    username: "test",
    password: "test",
    database: "test"
});
```

* `createConnections` 创建多个连接：

```typescript
import {createConnections, Connection} from "typeorm";

const connections = await createConnections([{
    name: "default",
    type: "mysql",
    host: "localhost",
    port: 3306,
    username: "test",
    password: "test",
    database: "test"
}, {
    name: "test2-connection",
    type: "mysql",
    host: "localhost",
    port: 3306,
    username: "test",
    password: "test",
    database: "test2"
}]);
```

这两个函数都基于传递的连接选项创建 `Connection`，并调用 `connect` 方法。

你可以在你的项目根目录创建 [ormconfig.json](./using-ormconfig.md) 文件，如果没有指定连接选项，这两种方法都可以自动地从`ormconfig`文件读取配置。

与 `node_modules` 同级的目录为项目的根目录。

例如：

```typescript
import {createConnection, Connection} from "typeorm";

// 这里 createConnection 将从
// ormconfig.json / ormconfig.js / ormconfig.yml / ormconfig.env / ormconfig.xml
// 文件或是特定的环境变量里载入连接选项
const connection: Connection = await createConnection();

// 或者你可以指定要创建连接的名称
// 如果省略了名称，它将创建一个没有指定名称的连接。
const secondConnection: Connection = await createConnection("test2-connection");

// 如果使用createConnections来代替createConnection，则会初始化并返回ormconfig文件中定义的所有连接
const connections: Connection[] = await createConnections();
```

不同的连接必须有不同的名称。

默认情况下，如果没有指定连接名称，它就等于 `default`。

通常，当你有多个数据库或多个连接配置时，你会使用多个连接。

一旦你创建了一个连接，你就可以从你的应用程序的任何地方获得它，使用函数 `getConnection`：

```typescript
import {getConnection} from "typeorm";

// 当`createConnection`被成功调用之后就可以使用
const connection = getConnection();

// 如果你有多连接你可以通过名称来得到连接;
const secondConnection = getConnection("test2-connection");
```

避免创建额外的类或服务来存储和管理你的连接。

这个功能已经嵌入到TypeORM中了。

你不需要再额外设计和创建无用的逻辑。

## 使用 `ConnectionManager`

你可以使用 `ConnectionManager` 类创建连接，例如：

```typescript
import {getConnectionManager, ConnectionManager, Connection} from "typeorm";

const connectionManager = getConnectionManager();

const connection = connectionManager.create({
    type: "mysql",
    host: "localhost",
    port: 3306,
    username: "test",
    password: "test",
    database: "test",
});
await connection.connect(); // performs connection
```

这不是创建连接的普遍方法，但它可能对某些用户有用。

例如用户想创建连接并存储其实例，但是必须在实际“连接”建立的时候进行控制。

你还可以创建并维护自己的 `ConnectionManager`：

```typescript
import {getConnectionManager, ConnectionManager, Connection} from "typeorm";

const connectionManager = new ConnectionManager();

const connection = connectionManager.create({
    type: "mysql",
    host: "localhost",
    port: 3306,
    username: "test",
    password: "test",
    database: "test",
});
await connection.connect(); // performs connection
```

但是注意，这种方式你就不能使用 `getConnection` 了——你必须存储你的连接管理器实例并在需要的时候使用 `connectionManager.get` 获取一个连接。

通常要避免这种方法，以避免在应用程序中出现不必要的麻烦，仅当你必须要使用的时候才使用 `ConnectionManager`。

## 使用连接

一旦你的连接建立起来，你就可以在你的应用程序的任何地方通过 `getConnection` 使用它。

例如：

```typescript
import {getConnection} from "typeorm";
import {User} from "../entity/User";

export class UserController {
    
    @Get("/users")
    getAll() {
        return getConnection().manager.find(User);
    }
    
}
```

你也可以使用 `ConnectionManager#get` 获取一个连接，

但是在大多数情况下使用 `getConnection()` 就足够了。

连接与实体一起使用，特别是使用 `EntityManager` 和 `Repository`。

了解更多有关他们的信息请看 [实体管理器和存储库](working-with-entity-manager.md)。

但是通常来说，你不会使用太多 `Connection`。

大多数情况下，你只创建一个连接并使用 `getRepository()` 和 `getManager()`  来访问连接管理器和存储库，而无需直接使用连接对象。
例如：

```typescript
import {getManager, getRepository} from "typeorm";
import {User} from "../entity/User";

export class UserController {
    
    @Get("/users")
    getAll() {
        return getManager().find(User);
    }
    
    @Get("/users/:id")
    getAll(@Param("id") userId: number) {
        return getRepository(User).findOne(userId);
    }
    
}
```
