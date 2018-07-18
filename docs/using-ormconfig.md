# 使用 ormconfig.json

* [从配置文件创建一个新的连接](#从配置文件创建一个新的连接)
* [使用 `ormconfig.json`](#使用-ormconfig.json)
* [使用 `ormconfig.js`](#使用-ormconfig.js)
* [使用环境变量](#使用环境变量)
* [使用 `ormconfig.yml`](#使用-ormconfig.yml)
* [使用 `ormconfig.xml`](#使用-ormconfig.xml)
* [覆盖在ormconfig中定义的选项](#覆盖在ormconfig中定义的选项)

## 从配置文件创建一个新的连接

大多数情况下，你希望将连接选项存储在单独的配置文件中。
它使得方便和容易管理。
TypeORM 支持多重配置资源。
你只需要创建一个 `ormconfig.[format]` 文件在你应用的根目录（靠近 `package.json`）,
把你的配置放在那儿，然后在你的应用调用 `createConnection()` ，不用传任何配置。

```typescript
import {createConnection} from "typeorm";

// createConnection method will automatically read connection options from your ormconfig file
const connection = await createConnection();
```

支持的 ormconfig 文件格式有： `.json`, `.js`, `.env`, `.yml` 和 `.xml`。
 
## 使用 `ormconfig.json`

在你的项目根目录（靠近 `package.json`）创建 `ormconfig.json`。 内容如下:

```json
{
   "type": "mysql",
   "host": "localhost",
   "port": 3306,
   "username": "test",
   "password": "test",
   "database": "test"
}
```

你可以指定其它 [`ConnectionOptions`](./connection-options.md) 里面的选项。

如果您想创建多个连接，那么只需把它变成一个数组：

```json
[{
   "name": "default",
   "type": "mysql",
   "host": "localhost",
   "port": 3306,
   "username": "test",
   "password": "test",
   "database": "test"
}, {
   "name": "second-connection",
   "type": "mysql",
   "host": "localhost",
   "port": 3306,
   "username": "test",
   "password": "test",
   "database": "test"
}]
```

## 使用 `ormconfig.js`

在你的项目根目录（靠近 `package.json`）创建 `ormconfig.js`。内容如下:

```javascript
module.exports = {
   "type": "mysql",
   "host": "localhost",
   "port": 3306,
   "username": "test",
   "password": "test",
   "database": "test"
}
```

你可以指定其它 [`ConnectionOptions`](./connection-options.md) 里面的选项。
如果您想创建多个连接，那么只需把它变成一个数组，并返回这个数组。

## 使用 `ormconfig.env`

在你的项目根目录（靠近 `package.json`）创建 `.env` 或 `ormconfig.env`。 内容如下:

```ini
TYPEORM_CONNECTION = mysql
TYPEORM_HOST = localhost
TYPEORM_USERNAME = root
TYPEORM_PASSWORD = admin
TYPEORM_DATABASE = test
TYPEORM_PORT = 3000
TYPEORM_SYNCHRONIZE = true
TYPEORM_LOGGING = true
TYPEORM_ENTITIES = entity/.*js,modules/**/entity/.*js
```

可以设置的可用环境变量列表：

* TYPEORM_CONNECTION
* TYPEORM_HOST
* TYPEORM_USERNAME
* TYPEORM_PASSWORD
* TYPEORM_DATABASE
* TYPEORM_PORT
* TYPEORM_URL
* TYPEORM_SID
* TYPEORM_SCHEMA
* TYPEORM_SYNCHRONIZE
* TYPEORM_DROP_SCHEMA
* TYPEORM_MIGRATIONS_RUN
* TYPEORM_ENTITIES
* TYPEORM_MIGRATIONS
* TYPEORM_SUBSCRIBERS
* TYPEORM_ENTITY_SCHEMAS
* TYPEORM_LOGGING
* TYPEORM_LOGGER
* TYPEORM_ENTITY_PREFIX
* TYPEORM_MAX_QUERY_EXECUTION_TIME
* TYPEORM_ENTITIES_DIR
* TYPEORM_MIGRATIONS_DIR
* TYPEORM_SUBSCRIBERS_DIR
* TYPEORM_DRIVER_EXTRA

`ormconfig.env` 只应该在开发期间使用。
在生产中你可以将这些参数设置在真实的环境变量里面。

使用 `env` 文件或者环境变量你不能定义多个连接。
如果你的应用有多个连接请使用其它配置存储格式。

## 使用 `ormconfig.yml`

在你的项目根目录（靠近 `package.json`）创建 `ormconfig.yml`。 内容如下:

```yaml
default: # default connection
    host: "localhost"
    port: 3306
    username: "test"
    password: "test"
    database: "test"
    
second-connection: # other connection
    host: "localhost"
    port: 3306
    username: "test"
    password: "test"
    database: "test2"
```

你可以使用任何可用的连接选项。

## 使用 `ormconfig.xml`

在你的项目根目录（靠近 `package.json`）创建 `ormconfig.xml`。 内容如下:

```xml
<connections>
    <connection type="mysql" name="default">
        <host>localhost</host>
        <username>root</username>
        <password>admin</password>
        <database>test</database>
        <port>3000</port>
        <logging>true</logging>
    </connection>
    <connection type="mysql" name="second-connection">
        <host>localhost</host>
        <username>root</username>
        <password>admin</password>
        <database>test2</database>
        <port>3000</port>
        <logging>true</logging>
    </connection>
</connections>
```

你可以使用任何可用的连接选项。

## 覆盖在ormconfig中定义的选项

有时你可能想覆盖 `ormconfig` 文件中定义的选项，或者你想在你的配置项中加入一些 TypeScript / JavaScript 的逻辑。

在这种情况下，你可以从 `ormconfig` 文件加载选项，获取到 `ConnectionOptions`，然后你就可以在 将它传递给`createConnection`函数之前修改这些选项了：

```typescript
// read connection options from ormconfig file (or ENV variables)
const connectionOptions = await getConnectionOptions();

// do something with connectionOptions,
// for example append a custom naming strategy or a custom logger
Object.assign(connectionOptions, { namingStrategy: new MyNamingStrategy() });

// create a connection using modified connection options
const connection = await createConnection(connectionOptions);
```