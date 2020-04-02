## Redis
![](./Pics/信息存储.png)
- **REmote DIctionary Server**  
1. `C`语言开发的开源的高性能键值对数据库  
2. 内部采用单线程机制进行工作  
- **常用指令**   
1. 信息添加：`set key value`，如`set name pkz`  
2. 信息查询：`get key`  
3. 获取命令帮助文档：`help 命令名称`，`help @组名`
- **Redis数据类型**  
1. `Redis`自身是一个`Map`，所有数据都采用`key:value`的形式存储  
2. 数据类型指的是存储的数据的类型，也就是`value`部分的类型，`key`部分是字符串  
- **string**  
1. 存储单个数据，是最简单的数据存储类型，也是最常用的数据存储类型  
2. 一个存储空间保存一个数据  
3. 存储的内容通常为字符串，如果字符串以整数的形式展示，可以作为数字操作使用   
4. 基本操作：  
```
  添加/修改数据
  set key value
  
  获取数据
  get key
  
  删除数据
  del key
  
  添加/修改多个数据
  mset key1 value1 key2 value2 ...
  
  获取多个数据
  mget key1 key2 ...
  
  获取数据字符个数（字符串长度）
  strlen key
  
  追加信息到原始信息后面（如果原始信息存在就追加，否则新建）
  append key value
  
  设置数值数据增加指定范围的值
  incr key
  incrby key increment
  incrbyfloat key increment
  
  设置数值数据减少指定范围的值
  decr key
  decrby key increment
  
  设置数据具有指定的生命周期
  setex key seconds value
  psetex key milliseconds value
  
  数据操作不成功的反馈与数据正常操作之间的差异
  1) 表示运行结果是否成功
  (integer)0 -> false 失败
  (integer)1 -> true 成功
  2) 表示运行结果值
  (integer)3 -> 3个
  (integer)1 -> 1个
  数据未获取到
  (nil) 等用于null
  数据计算最大范围
  Java中的long的最大值
```  
5. `string`在`redis`内部存储默认就是一个字符串，当遇到增减类操作`incr`，`decr`时会转成数值型进行计算  
6. `redis`所有操作都是原子性的，采用单线程处理所有业务，命令是一个一个执行的，因此无需考虑并发带来的数据影响  
7. 按数值进行操作的数据，如果原始数据不能转成数值，或超过了`redis`数值上限范围，将报错  
![](./Pics/单指令多指令.png)
8. 数据库中热点数据`key`命名习惯：`表名:主键名:主键值:字段名` 
```
  set user:id:00789:fans 123456789
  set user:id:00789:blogs 789
  
  set user:id:00789 {id:00789,blogs:789,fans:123456789}
```
- **hash**  
1. 对一系列存储的数据进行编组，方便管理  
2. 一个存储空间保存多个键值对数据   
3. 底层使用哈希表结构实现数据存储  
![](./Pics/Redis_hash.png)  
4. 基本操作  
```
  添加/修改数据
  hset key field value
  
  获取数据
  hget key field
  hgetall key
  
  删除数据
  hdel key field1 [field2]
  
  添加/修改多个数据
  hmset key field1 value1 field2 value2 ...
  
  获取多个数据
  hmget key field1 field2 ...
  
  获取哈希表中字段的数量
  hlen key
  
  获取哈希表中是否存在指定的字段
  hexists key field
  
  获取哈希表中所有的字段名或字段值
  hkeys key
  hvals key
  
  设置指定字段的数值数据增加指定范围的值
  hincrby key field increment
  hincrbyfloat key field increment
  
  如果字段已经存在于哈希表中，操作无效
  hsetnx key field value
```  
- **list**  
1. 存储多个数据，并对数据进入存储空间的顺序进行区分  
2. 一个存储空间保存多个数据，且通过数据可以体现进入顺序  
3. 保存多个数据，底层使用双向链表存储结构实现  
![](./Pics/Redis_list.png)  
4. 基本操作  
```
  添加/修改数据  
  lpush key value1 [value2] ...
  rpush key value1 [value2] ...
  
  获取数据
  lrange key start stop
  lindex key index
  llen key
  
  获取并移除数据
  lpop key
  rpop key
  
  规定时间内获取并移除数据
  blpop key1 [key2] timeout
  brpop key1 [key2] timeout
  
  移除指定（个数）数据
  lrem key count value
```  
- **set**  
1. 存储大量的数据，在查询方面提供更高的效率  
2. 与`hash`存储结构完全相同，仅存储键，不存储值（`nil`），并且值不允许重复   
![](./Pics/Redis_set.png)  
3. 基本操作  
```
  添加数据  
  sadd key member1 [member2]  
  
  获取全部数据
  smembers key
  
  删除数据
  srem key member1 [member2]
  
  获取集合数据总量
  scard key  
  
  判断集合中是否包含指定数据
  sismember key member
  
  随机获取集合中指定数量的数据
  srandmember key [count]
  
  随机获取集合中的某个数据并将该数据移出集合
  spop key
  
  求两个集合的交、并、差集
  sinter key1 [key2]
  sunion key1 [key2]
  sdiff key1 [key2]
  
  求两个集合的交、并、差集并存储到指定集合中
  sinterstore destination key1 [key2]
  sunionstore destination key1 [key2]
  sdiffstore destination key1 [key2]
  
  将指定数据从原始集合中移动到目标集合中
  smove source destination member
```  
4. `set`类型不允许数据重复，如果添加的数据在`set`中已经存在，将只保留一份  
5. `set`虽然与`hash`的存储结构相同，但是无法启用`hash`中存储值的空间  
- **sorted_set** 
1. 数据排序有利于数据的有效展示，需要提供一种根据自身特征进行排序的方式  
2. 在`set`的存储结构基础上添加可排序字段  
![](./Pics/Redis_sorted_set.png)  
3. 基本操作  
```
  添加数据
  zadd key score1 member1 [score2 member2]
  
  获取全部数据
  zrange key start stop [WITHSCORES]
  zrevrange key start stop [WITHSCORES]
  
  删除数据
  zrem key member [member ...]
  
  按条件获取数据
  zrangebyscore key min max [WITHSCORES] [LIMIT]
  zrevrangebyscore key max min [WITHSCORES]
  
  条件删除数据
  zremrangebyrank key start stop
  zremrangebyscore key min max
  
  min与max用于限定搜索查询的条件
  start与stop用于限定查询范围，作用于索引，表示开始和结束索引
  offset与count用于限定查询范围，作用于查询结果，表示开始位置和数据总量
  
  获取集合数据总量
  zcard key
  zcount key min max
  
  集合交、并操作
  numkeys表示key的数量
  zinterstore destination numkeys key [key ...]
  zunionstore destination numkeys key [key ...]
  
  获取数据对应的索引（排名）
  zrank key member
  zrevrank key member
  
  score值获取与修改
  zscore key member
  zincrby key increment member  
  
  获取当前系统时间
  time
```
4. `sorted_set`底层存储还是基于`set`结构，数据不能重复，如果重复添加相同的数据，`score`值将被反复覆盖，保留最后一次修改的结果
- **限制单用户单位时间内访问次数**  
1. 解决方案：  
a. 设计计数器，记录调用次数，用于控制业务执行次数。以用户`id`作为`key`，访问次数作为`value`  
b. 在访问之前获取次数，判断是否超过限定次数：不超过，则每次访问`计数 + 1`；访问失败，`计数 - 1`  
c. 为计数器设置生命周期为指定单位时间，自动清空周期内访问次数  
2. 解决方案改良：  
a. 取消最大值的判定，利用`incr`操作超过最大值抛出异常的形式，替代每次判断是否大于最大值  
b. 访问之前判断是否为`nil`：如果是，设置计数值为`Max - 限制访问次数`；如果不是，`计数 + 1`；访问失败，`计数 - 1`  
c. 遇到异常，则视为访问次数达到上限
