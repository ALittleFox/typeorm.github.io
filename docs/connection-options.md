# 连接选项

* [什么是`ConnectionOptions`](#what-is-connectionoptions)
* [通用连接选项](#common-connection-options)
* [`mysql` / `mariadb` 连接选项](#mysql--mariadb-connection-options)
* [`postgres` 连接选项](#postgres-connection-options)
* [`sqlite` 连接选项](#sqlite-connection-options)
* [`websql` 连接选项](#websql-connection-options)
* [`cordova` 连接选项](#cordova-connection-options)
* [`mssql` 连接选项](#mssql-connection-options)
* [`mongodb` 连接选项](#mongodb-connection-options)
* [`sql.js` 连接选项](#sqljs-connection-options)
* [连接选项示例](#connection-options-example)
    
## 什么是`ConnectionOptions`

连接选项是您传递给`createConnection`或在`ormconfig`文件中创建的连接配置对象。不同的数据库驱动程序有它们自己的特定连接选项。

## 通用连接选项

* `type` - 数据库类型。你必须申明你使用的数据库类型。可选的值有`mysql`、`postgres`、`mariadb`、`sqlite`、`cordova`、`oracle`、`mssql`、`websql`、`mongodb`、`sqljs`。这个选项是必须的。

* `name` - 连接名。你将使用 `getConnection(name: string)` 或 `ConnectionManager.get(name: string)` 来获取你需要使用的连接。
不同连接的连接名称不能相同 — 它们都必须是惟一的。
如果没有连接名称，那么它将被称为`default`。

* `extra` - 要传递给底层数据库驱动程序的额外连接选项。
如果您想要将额外的选项传递给底层数据库驱动程序，请使用它。

* `entities` - 要加载和用于此连接的实体。
支持通过实体类名和目录路径来加载。
目录路径支持 `glob` 模式。
例如：`entities: [Post, Category, "entity/*.js", "modules/**/entity/*.js"]`。
了解更多关于[实体](./entities.md)。

* `subscribers` - 要加载和用于此连接的订阅者。
支持通过类名和目录路径来加载。
目录路径支持 `glob` 模式。
例如：`subscribers: [PostSubscriber, AppSubscriber, "subscriber/*.js", "modules/**/subscriber/*.js"]`。
了解更多关于[订阅者](listeners-and-subscribers.md)。

* `entitySchemas` - 要加载和用于此连接的实体模式。
支持通过实体模式类名和目录路径来加载。
目录路径支持 `glob` 模式。
例如：`entitySchemas: [PostSchema, CategorySchema, "entity-schema/*.json", "modules/**/entity-schema/*.json"]`。
了解更多关于[实体模式](./schema-in-files.md)。

* `migrations` - 要加载并用于此连接的数据库迁移。
支持通过迁移类名和目录路径来加载。
目录路径支持 `glob` 模式。
例如：`migrations: [FirstMigration, SecondMigration, "migration/*.js", "modules/**/migration/*.js"]`。
了解更多关于[迁移](./migrations.md)。

* `logging` - 是否启用日志记录。
设置为 `true` 将开启查询和错误日志。
你还可以指定启用不同类型的日志记录，例如：`["query", "error", "schema"]`.
了解更多关于[日志](./logging.md).

* `logger` - 用于日志记录的日志记录器。 可以选值有 `advanced-console`、`simple-console`和`file`。
默认是`advanced-console`。你也可以通过实现`Logger`接口来指定自己的日志记录类。
了解更多关于[日志](./logging.md)。

* `maxQueryExecutionTime` - 如果查询执行时间超过给定的最大执行时间(以毫秒为单位)，那么日志记录器将记录这个查询。

* `namingStrategy` - 用于在数据库中命名表和列的命名策略。
了解更多关于[命名策略](./naming-strategy.md).

* `entityPrefix` - 此数据库连接的所有表(或集合)名称的前缀。

* `dropSchema` - 每次建立连接时删除模式。
小心这个选项，不要在生产中使用它 — 否则你将失去所有的生产数据。
此选项在调试和开发期间非常有用。

* `synchronize` - 是否应该在每个应用程序启动时自动创建数据库模式。
小心这个选项，不要在生产中使用它 — 否则你将失去所有的生产数据。
此选项在调试和开发期间非常有用。
作为替代方案，你可以通过命令行工具执行 `schema:sync` 命令来同步。
注意，对于MongoDB数据库，它不创建模式，因为MongoDB是无模式的。
相反，它只是通过创建索引来实现同步。

* `migrationsRun` - 是否应该在每次应用程序启动时自动运行迁移。
作为替代方案，你可以通过命令行工具执行 `migrations:run` 命令。

* `cli.entitiesDir` - 通过命令行工具创建的实体的存放目录。

* `cli.migrationsDir` - 通过命令行工具创建的迁移的存放目录。

* `cli.subscribersDir` - 通过命令行工具创建的订阅者的存放目录。

## `mysql` / `mariadb` 连接选项

* `url` - 数据库连接地址。

* `host` - 数据库主机。

* `port` - 数据库主机端口。 默认 `3306`.

* `username` - 数据库用户名。

* `password` - 数据库密码

* `database` - 数据库名

* `charset` - 连接字符集。在MySQL中称为字符集校对规则（像`utf8_general_ci`）。如果设置了字符集（如`utf8mb4`），那么就会使用那个字符集默认的校对规则。（默认：`UTF8_GENERAL_CI`）。

* `timezone` - MySQL服务器的时区。这用于将服务器日期/时间值与JavaScript日期对象互相匹配。可以是`local`、`Z`或者是`+HH:MM` 或 `-HH:MM`格式的偏移量。（默认：`local`）

* `connectTimeout` - MySQL服务器的连接超时时间。（默认：`10000`）
 
* `insecureAuth` - 允许通过旧（不安全）的身份验证方法连接到MySQL实例。（默认：`false`）
 
* `supportBigNumbers` - 处理数据库里面的大型数字（`BIGINT` 和 `DECIMAL` 列），您应该启用这个选项。（默认：`false`）
 
* `bigNumberStrings` - 同时启用 `supportBigNumbers` 和 `bigNumberStrings` 可以强制将大数字（`BIGINT`和`DECIMAL`列）作为JavaScript字符串对象返回（默认为`false`）。
只启用 `supportBigNumbers` 但禁用 `bigNumberStrings` 只会在大型数字不能被准确地表示出来[JavaScript 数字对象](http://ecma262-5.com/ELS5_HTML.htm#Section_8.5)（只有当它们超出`[-2^53, +2^53]`范围后才会发生）时才会被转成字符串对象，否则它们将作为数字对象返回。如果`supportBigNumbers`被禁用，此选项将被忽略。

* `dateStrings` - 强制日期类型（`TIMESTAMP`、`DATETIME `、`date`）作为字符串返回，而不是转换成到JavaScript日期对象。 可以是 `true` 或 `false` 或者是一个需要作为字符串的类型名称数组。（默认：`false`）

* `debug` - 打印连接详细信息到标准输出。可以是 `true` 或 `false` 或者是一个需要打印的包类型名称的数组。（默认：`false`）

* `trace` - 在发生错误的时候生成堆栈跟踪，包括库入口的调用位置。（“长堆栈跟踪”）。 
对于大多数请求都会有轻微的性能损失。（默认：`true`）

* `multipleStatements` - Allow multiple mysql statements per query. Be careful with this, it could increase the scope 
of SQL injection attacks. (Default: `false`)

* `flags` - List of connection flags to use other than the default ones. It is also possible to blacklist default ones.
 For more information, check [Connection Flags](https://github.com/mysqljs/mysql#connection-flags).
 
* `ssl` -  object with ssl parameters or a string containing the name of ssl profile. 
See [SSL options](https://github.com/mysqljs/mysql#ssl-options).

## `postgres` connection options

* `url` - Connection url where perform connection to.

* `host` - Database host.

* `port` - Database host port. Default postgres port is `5432`.

* `username` - Database username.

* `password` - Database password.

* `database` - Database name.

* `schema` - Schema name. Default is "public".

* `ssl` - Object with ssl parameters. See [TLS/SSL](https://node-postgres.com/features/ssl).

## `sqlite` connection options

* `database` - Database path. For example "./mydb.sql"

## `websql` connection options

* `database` - Database name

* `version` - Version string of the database

* `description` - Database description

* `size` - The size of the database

## `cordova` connection options

* `database` - Database name

* `location` - Where to save the database. See [cordova-sqlite-storage](https://github.com/litehelpers/Cordova-sqlite-storage#opening-a-database) for options.

## `mssql` connection options

* `url` - Connection url where perform connection to.

* `host` - Database host.

* `port` - Database host port. Default mssql port is `1433`.

* `username` - Database username.

* `password` - Database password.

* `database` - Database name.

* `schema` - Schema name. Default is "public".

* `domain` - Once you set domain, the driver will connect to SQL Server using domain login.

* `connectionTimeout` - Connection timeout in ms (default: `15000`).

* `requestTimeout` - Request timeout in ms (default: `15000`). NOTE: msnodesqlv8 driver doesn't support
 timeouts < 1 second.

* `stream` - Stream recordsets/rows instead of returning them all at once as an argument of callback (default: `false`).
 You can also enable streaming for each request independently (`request.stream = true`). Always set to `true` if you plan to
 work with a large amount of rows.
 
* `pool.max` - The maximum number of connections there can be in the pool (default: `10`).

* `pool.min` - The minimum of connections there can be in the pool (default: `0`).

* `pool.maxWaitingClients` - maximum number of queued requests allowed, additional acquire calls will be callback with
 an err in a future cycle of the event loop.
 
* `pool.testOnBorrow` -  should the pool validate resources before giving them to clients. Requires that either
  `factory.validate` or `factory.validateAsync` to be specified.
  
* `pool.acquireTimeoutMillis` - max milliseconds an `acquire` call will wait for a resource before timing out.
 (default no limit), if supplied should non-zero positive integer.
 
* `pool.fifo` - if true the oldest resources will be first to be allocated. If false the most recently released resources
 will be the first to be allocated. This in effect turns the pool's behaviour from a queue into a stack. boolean,
 (default `true`).
 
* `pool.priorityRange` - int between 1 and x - if set, borrowers can specify their relative priority in the queue if no
 resources are available. see example. (default `1`).
 
* `pool.autostart` - boolean, should the pool start creating resources etc once the constructor is called, (default `true`).

* `pool.victionRunIntervalMillis` - How often to run eviction checks. Default: `0` (does not run).

* `pool.numTestsPerRun` - Number of resources to check each eviction run. Default: `3`.

* `pool.softIdleTimeoutMillis` - amount of time an object may sit idle in the pool before it is eligible for eviction by
 the idle object evictor (if any), with the extra condition that at least "min idle" object instances remain in the pool.
 Default `-1` (nothing can get evicted).
 
* `pool.idleTimeoutMillis` -  the minimum amount of time that an object may sit idle in the pool before it is eligible for
 eviction due to idle time. Supersedes `softIdleTimeoutMillis`. Default: `30000`.
 
* `options.fallbackToDefaultDb` - By default, if the database requestion by `options.database` cannot be accessed, the connection
 will fail with an error. However, if `options.fallbackToDefaultDb` is set to `true`, then the user's default database will
  be used instead (Default: `false`).
  
* `options.enableAnsiNullDefault` - If true, SET ANSI_NULL_DFLT_ON ON will be set in the initial sql. This means new
 columns will be nullable by default. See the [T-SQL documentation](https://msdn.microsoft.com/en-us/library/ms187375.aspx)
 for more details. (Default: `true`).
 
* `options.cancelTimeout` - The number of milliseconds before the cancel (abort) of a request is considered failed (default: `5000`).

* `options.packetSize` - The size of TDS packets (subject to negotiation with the server). Should be a power of 2. (default: `4096`).

* `options.useUTC` - A boolean determining whether to pass time values in UTC or local time. (default: `true`).

* `options.abortTransactionOnError` - A boolean determining whether to rollback a transaction automatically if any
 error is encountered during the given transaction's execution. This sets the value for `SET XACT_ABORT` during the
 initial SQL phase of a connection ([documentation](http://msdn.microsoft.com/en-us/library/ms188792.aspx)).

* `options.localAddress` - A string indicating which network interface (ip address) to use when connecting to SQL Server.

* `options.useColumnNames` - A boolean determining whether to return rows as arrays or key-value collections. (default: `false`).

* `options.camelCaseColumns` - A boolean, controlling whether the column names returned will have the first letter
 converted to lower case (`true`) or not. This value is ignored if you provide a `columnNameReplacer`. (default: `false`).

* `options.isolationLevel` - The default isolation level that transactions will be run with. The isolation levels are
 available from `require('tedious').ISOLATION_LEVEL`.
   * `READ_UNCOMMITTED`
   * `READ_COMMITTED`
   * `REPEATABLE_READ`
   * `SERIALIZABLE`
   * `SNAPSHOT`
   
   (default: `READ_COMMITTED`)
 
* `options.connectionIsolationLevel` - The default isolation level for new connections. All out-of-transaction queries
 are executed with this setting. The isolation levels are available from `require('tedious').ISOLATION_LEVEL`.
   * `READ_UNCOMMITTED`
   * `READ_COMMITTED`
   * `REPEATABLE_READ`
   * `SERIALIZABLE`
   * `SNAPSHOT`
   
   (default: `READ_COMMITTED`)
 
* `options.readOnlyIntent` - A boolean, determining whether the connection will request read-only access from a
 SQL Server Availability Group. For more information, see here. (default: `false`).

* `options.encrypt` - A boolean determining whether or not the connection will be encrypted. Set to true if you're
 on Windows Azure. (default: `false`).
 
* `options.cryptoCredentialsDetails` - When encryption is used, an object may be supplied that will be used for the
 first argument when calling [tls.createSecurePair](http://nodejs.org/docs/latest/api/tls.html#tls_tls_createsecurepair_credentials_isserver_requestcert_rejectunauthorized)
  (default: `{}`).
  
* `options.rowCollectionOnDone` - A boolean, that when true will expose received rows in Requests' `done*` events.
 See done, [doneInProc](http://tediousjs.github.io/tedious/api-request.html#event_doneInProc)
 and [doneProc](http://tediousjs.github.io/tedious/api-request.html#event_doneProc). (default: `false`)
   
   Caution: If many rows are received, enabling this option could result in excessive memory usage.

* `options.rowCollectionOnRequestCompletion` - A boolean, that when true will expose received rows
 in Requests' completion callback. See [new Request](http://tediousjs.github.io/tedious/api-request.html#function_newRequest). (default: `false`)

   Caution: If many rows are received, enabling this option could result in excessive memory usage.

* `options.tdsVersion` - The version of TDS to use. If the server doesn't support the specified version, a negotiated version
 is used instead. The versions are available from `require('tedious').TDS_VERSION`.
   * `7_1`
   * `7_2`
   * `7_3_A`
   * `7_3_B`
   * `7_4`
   
  (default: `7_4`)

* `options.debug.packet` - A boolean, controlling whether `debug` events will be emitted with text describing packet
 details (default: `false`).
 
* `options.debug.data` - A boolean, controlling whether `debug` events will be emitted with text describing packet data
 details (default: `false`).
 
* `options.debug.payload` - A boolean, controlling whether `debug` events will be emitted with text describing packet
 payload details (default: `false`).
 
* `options.debug.token` - A boolean, controlling whether `debug` events will be emitted with text describing token stream
 tokens (default: `false`).

## `mongodb` connection options

* `url` - Connection url where perform connection to.

* `host` - Database host.

* `port` - Database host port. Default mongodb port is `27017`.

* `database` - Database name.

* `poolSize` - Set the maximum pool size for each individual server or proxy connection.

* `ssl` - Use ssl connection (needs to have a mongod server with ssl support). Default: `false`.

* `sslValidate` - Validate mongod server certificate against ca (needs to have a mongod server with ssl support,
 2.4 or higher). Default: `true`.
 
* `sslCA` - Array of valid certificates either as Buffers or Strings (needs to have a mongod server with ssl support,
 2.4 or higher).
 
* `sslCert` - String or buffer containing the certificate we wish to present (needs to have a mongod server with ssl
 support, 2.4 or higher)
 
* `sslKey` - String or buffer containing the certificate private key we wish to present (needs to have a mongod server
 with ssl support, 2.4 or higher).
 
* `sslPass` - String or buffer containing the certificate password (needs to have a mongod server with ssl support,
 2.4 or higher).
 
* `autoReconnect` - Reconnect on error. Default: `true`.

* `noDelay` - TCP Socket NoDelay option. Default: `true`.

* `keepAlive` - The number of milliseconds to wait before initiating keepAlive on the TCP socket. Default: `30000`.

* `connectTimeoutMS` - TCP Connection timeout setting. Default: `30000`.

* `socketTimeoutMS` - TCP Socket timeout setting. Default: `360000`.

* `reconnectTries` - Server attempt to reconnect #times. Default: `30`.

* `reconnectInterval` - Server will wait #milliseconds between retries. Default: `1000`.

* `ha` - Turn on high availability monitoring. Default: `true`.

* `haInterval` - Time between each replicaset status check. Default: `10000,5000`.

* `replicaSet` - The name of the replicaset to connect to.
 
* `acceptableLatencyMS` - Sets the range of servers to pick when using NEAREST (lowest ping ms + the latency fence,
 ex: range of 1 to (1 + 15) ms). Default: `15`.

* `secondaryAcceptableLatencyMS` - Sets the range of servers to pick when using NEAREST (lowest ping ms + the latency
 fence, ex: range of 1 to (1 + 15) ms). Default: `15`.
 
* `connectWithNoPrimary` - Sets if the driver should connect even if no primary is available. Default: `false`.

* `authSource` - If the database authentication is dependent on another databaseName.

* `w` - The write concern.

* `wtimeout` - The write concern timeout value.

* `j` - Specify a journal write concern. Default: `false`.

* `forceServerObjectId` - Force server to assign _id values instead of driver. Default: `false`.

* `serializeFunctions` - Serialize functions on any object. Default: `false`.

* `ignoreUndefined` - Specify if the BSON serializer should ignore undefined fields. Default: `false`.

* `raw` - Return document results as raw BSON buffers. Default: `false`.

* `promoteLongs` - Promotes Long values to number if they fit inside the 53 bits resolution. Default: `true`.

* `promoteBuffers` - Promotes Binary BSON values to native Node Buffers. Default: `false`.

* `promoteValues` - Promotes BSON values to native types where possible, set to false to only receive wrapper types.
 Default: `true`.
 
* `domainsEnabled` - Enable the wrapping of the callback in the current domain, disabled by default to avoid perf hit.
 Default: `false`.
 
* `bufferMaxEntries` - Sets a cap on how many operations the driver will buffer up before giving up on getting a
 working connection, default is -1 which is unlimited.
 
* `readPreference` - The preferred read preference.
  * `ReadPreference.PRIMARY`
  * `ReadPreference.PRIMARY_PREFERRED`
  * `ReadPreference.SECONDARY`
  * `ReadPreference.SECONDARY_PREFERRED`
  * `ReadPreference.NEAREST`
  
* `pkFactory` - A primary key factory object for generation of custom _id keys.

* `promiseLibrary` - A Promise library class the application wishes to use such as Bluebird, must be ES6 compatible.

* `readConcern` - Specify a read concern for the collection. (only MongoDB 3.2 or higher supported).

* `maxStalenessSeconds` - Specify a maxStalenessSeconds value for secondary reads, minimum is 90 seconds.

* `appname` - The name of the application that created this MongoClient instance. MongoDB 3.4 and newer will print this
 value in the server log upon establishing each connection. It is also recorded in the slow query log and profile
 collections
 
* `loggerLevel` - Specify the log level used by the driver logger (`error/warn/info/debug`).

* `logger` - Specify a customer logger mechanism, can be used to log using your app level logger.

* `authMechanism` - Sets the authentication mechanism that MongoDB will use to authenticate the connection.

## `sql.js` connection options

* `database`: The raw UInt8Array database that should be imported.

* `autoSave`: Whether or not autoSave should be disabled. If set to true the database will be saved to the given file location (Node.js) or LocalStorage element (browser) when a change happens and `location` is specified. Otherwise `autoSaveCallback` can be used.

* `autoSaveCallback`: A function that get's called when changes to the database are made and `autoSave` is enabled. The function gets a `UInt8Array` that represents the database.

* `location`: The file location to load and save the database to.

## Connection options example

Here is a small example of connection options for mysql:

```typescript
{
    host: "localhost",
    port: 3306,
    username: "test",
    password: "test",
    database: "test",
    logging: true,
    synchronize: true,
    entities: [
        "entity/*.js"
    ],
    subscribers: [
        "subscriber/*.js"
    ],
    entitySchemas: [
        "schema/*.json"
    ],
    migrations: [
        "migration/*.js"
    ],
    cli: {
        entitiesDir: "entity",
        migrationsDir: "migration",
        subscribersDir: "subscriber"
    }
}
```