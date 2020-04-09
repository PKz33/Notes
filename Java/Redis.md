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
- **Redis案例**  
1. 分布式锁  
```
  SETNX product:101 true  // 返回1代表获取锁成功  
  SETNX product:101 false // 返回0代表获取锁失败
  // 执行业务操作
  DEL product:101 // 执行完业务释放锁
  
  SET product:101 true EX 10 NX // 防止程序意外终止导致死锁
  // EX seconds：将key的过期时间设置为seconds秒，等同于SETEX key seconds value
  // NX：只有key不存在时，才对key进行设置操作，等同于SETNX key value
```  
2. 计数器  
```
  INCR article:readcount:{文章id}
  GET article:readcount:{文章id}
```  
3. `Web`集群`session`共享
4. 分布式系统全局序列号：`INCRBY orderId 1000  // redis批量生成序列号提升性能`  
5. 电商购物车  
```
  1) 以用户id为key
  2) 商品id为field
  3) 商品数量为value
  
  购物车操作
  1) 添加商品：HSET cart:101 1001 1
  2) 增加数量：HINCRBY cart:101 1001 1
  3) 商品总数：HLEN cart:101
  4) 删除商品：HDEL cart:101 1001
  5) 获取购物车所有商品：HGETALL cart:101
```  
6. 模拟常用数据结构  
```
  Stack（栈） = LPUSH + LPOP
  Queue（队列） = LPUSH + RPOP
  Blocking MQ（阻塞队列） = LPUSH + BRPOP
```
7. 微博消息和微信公众号消息  
```
  A关注了B，C等大V
  1) B发微博，消息ID为101
  LPUSH msg:{A-ID} 101
  2) C发微博，消息ID为104
  LPUSH msg:{A-ID} 104
  3) 查看最新微博消息
  LRANGE msg:{A-ID} 0 5
```
8. 微信抽奖小程序
```
  1) 点击参与抽奖加入集合
  SADD key {userID}
  2) 查看参与抽奖的所有用户
  SMEMBERS key
  3) 抽取count名中奖者
  SRANDMEMBER key [count] // 抽取后保留
  SPOP key [count]  // 抽取后不保留  
```
9. 微信微博点赞，收藏，标签
```
  1) 点赞
  SADD like:{消息ID} {用户ID}
  2) 取消点赞
  SREM like:{消息ID} {用户ID}
  3) 检查用户是否点过赞
  SISMEMBER like:{消息ID} {用户ID}
  4) 获取点赞的用户列表
  SMEMBERS like:{消息ID}
  5) 获取点赞用户数
  SCARD like:{消息ID}
```  
10. 集合操作实现微博微信关注模型
```
  1) A关注的人：
  aSet  {B, C, D}
  2) B关注的人：
  bSet  {A, C, D, E}
  3) C关注的人：
  cSet  {A, B, E, D, F}
  4) A和B共同关注的人：
  SINTER aSet bSet  // {C, D}
  5) 我（A）关注的人也关注他（B）：
  SISMEMBER cSet B
  SISMEMBER dSet B
  6) 我（A）可能认识的人：
  SDIFF bSet aSet // {A, E}
```
11. `Zset`集合操作实现排行榜
```
  1) 点击新闻
  ZINCRBY hotNews:20200406 1 新闻1
  2) 展示当日排行前十
  ZREVRANGE hotNews:20200406 0 9 WITHSCORES
  3) 七日搜索榜单计算
  ZUNIONSORE hns:0331-0406 7 hns:0331 hns:0401 hns:0402 hns:0403 hns:0404 hns:0405 hns:0406
  4) 展示七日排行前十
  ZREVRANGE hns:0331-0406 0 9 WITHSCORES
```
- **key**  
1. `key`是一个字符串，通过`key`获取`redis`中保存的数据  
2. 基本操作  
```
  删除指定key
  DEL key
  
  获取key是否存在
  EXISTS key
  
  获取key的类型
  TYPE key
  
  为指定key设置有效期
  EXPIRE key seconds
  EXPIRE key milliseconds
  EXPIREAT key timestamp
  EXPIREAT key milliseconds-timestamp
  
  获取key的有效时间
  TTL key
  PTTL key
  
  切换key从时效性转换为永久性
  PERSIST key
  
  为key改名
  RENAME key newkey
  RENAMENX key newkey
  
  对所有key排序
  SORT
  
  其他key通用操作
  HELP @GENERIC
```
3. 查询模式
```
  查询key
  KEYS pattern
  
  查询模式规则
  * 匹配任意数量的任意符号
  ? 匹配一个任意符号
  [] 匹配一个指定符号
  
  查询所有
  KEYS * 
  
  查询以pkz开头
  KEYS pkz*
  
  查询以pkz结尾
  KEYS *pkz
  
  查询所有前面两个字符任意，后面以pkz结尾
  KEYS ??pkz
  
  查询所有以user:开头，最后一个字符任意
  KEYS user:?
  
  查询所有以u开头，以er:1结尾，中间包含一个字母，s或t
  KEYS u[st]er:1  
```
4. `redis`为每个服务提供16个数据库，编号从0到15，每个数据库之间的数据相互独立
```
  切换数据库
  SELECT index
  
  其他操作
  QUIT
  PING
  ECHO message
  
  数据移动
  MOVE key db
  
  数据清除
  DBSIZE
  FLUSHDB
  FLUSHALL
```
- **Jedis**  
![](./Pics/Jedis.png)
1. 客户端连接`redis`  
```
  <!-- 导入依赖 -->
  <dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
  </dependency>
  
  // 连接 redis
  Jedis jedis = new Jedis("localhost", 6379);
  
  // 操作redis
  jedis.set("name", "pkz");
  jedis.get("name");
  
  // 关闭redis连接
  jedis.close();
```
2. 服务调用次数控制案例  
```
  案例需求：
  1) 设定A、B、C三个用户
  2) A用户限制10次/分调用，B用户限制30次/分调用，C用户不限制
  
  // 设定业务方法
  public void business(String id, Long val){
    System.out.println("用户：" + id + " 业务操作执行第" + val + "次");
  }
  
  // 设定多线程类，模拟用户调用
  class MyThread extends Thread {
    Service sc;
    
    public MyThread(String id, int num){
      sc = new Service(id, num);
    }
    
    public void run(){
      while(true){
        sc.service();
        try{
          Thread.sleep(300L);
        }catch(InterruptedException e){
          e.printStackTrace();
        }
      }
    }
  }
  
  // 设计redis控制方案
  public void service(){
    Jedis jedis = new Jedis("127.0.0.1", 6379);
    String value = jedis.get("compid:" + id);
    // 判断该值是否存在
    try{
      if(value == null){
        // 不存在，创建该值
        jedis.setex("compid:" + id, 5, Long.MAX_VALUE - num + "");
      }else{
        // 存在，自增，调用业务
        Long val = jedis.incr("compid:" + id);
        business(id, num - (Long.MAX_VALUE - val));
      }
    }catch(JedisDataException e){
      System.out.println("访问达到次数上限，请升级会员级别");
      return;
    }finally{
      jedis.close();
    }
  }
  
  // 设计启动主程序
  public static void main(String[] args){
    MyThread mt1 = new MyThread("初级用户", 10);
    MyThread mt2 = new MyThread("高级用户", 30);
    mt1.start();
    mt2.start();
  }
  
  // 对业务控制方案进行改造，设定不同用户等级的判定
  // 将不同用户等级对应的信息、限制次数等设定到redis中，使用hash保存
```
3. 基于连接池获取连接
```
  JedisPool：Jedis提供的连接池技术
    poolConfig：连接池配置对象
    host：redis服务地址
    port：redis服务端口号
  public JedisPool(GenericObjectPoolConfig poolConfig, String host, int port){
    this(poolConfig, host, port, 2000, (String)null, 0, (String)null);
  }
  
  // 封装连接参数
  // jedis.properties
  jedis.host = localhost
  jedis.port = 6379
  jedis.maxTotal = 30
  jedis.maxIdle = 10
  
  // 加载配置信息
  // 静态代码块初始化资源
  static{
    // 读取配置文件，获取参数值
    ResourceBundle rb = ResourceBundle.getBundle("jedis");
    host = rb.getString("jedis.host");
    port = Integer.parseInt(rb.getString("jedis.port"));
    maxTotal = Integer.parseInt(rb.getString("jedis.maxTotal"));
    maxIdle = Integer.parseInt(rb.getString("jedis.maxIdle"));
    poolConfig = new JedisPoolConfig();
    poolConfig.setMaxTotal(maxTotal);
    poolConfig.setMaxIdle(maxIdle);
    jedisPool = new JedisPool(poolConfig, host, port);
  }
  
  // 获取连接
  // 对外访问接口，提供jedis连接对象，连接从连接池获取
  public static Jedis getJedis(){
    Jedis jedis = jedisPool.getResource();
    return jedis;
  }
```
- **Linux环境下安装redis**  
- **配置启动redis**  
```
  cat redis.conf | grep -v "#" | grep -v "^$" > redis-6379.conf
  
  cat redis-6379.conf
  # port 6379
  # daemonize yes
  # logfile "6379.log"
  # dir /root/redis-5.0.5/data
  
  redis-server redis-6379.conf
  
  ps -ef | grep redis-
  
  kill -s 9 进程号
  
  mkdir conf
  
  mv redis-6379.conf conf
  
  cp redis-6379.conf redis-6380.conf
  
  cat redis-6380.conf
  # 设定当前服务启动的端口号
  # port 6380
  # 以守护进程方式启动，redis将以服务的形式存在，日志将不再打印到命令窗口中
  # daemonize yes
  # 设定日志文件名，方便查阅
  # logfile "6380.log"
  # 设定当前服务文件保存位置，包含日志文件、持久化文件等
  # dir /root/redis-5.0.5/data
  
  默认配置启动
  redis-server
  redis-server --port 6379
  redis-server --port 6380
  
  指定配置文件启动
  redis-server redis.conf
  redis-server redis-6379.conf
  redis-server redis-6380.conf
  redis-server conf/redis-6379.conf
  redis-server config/redis-6380.conf
  
  Redis客户端连接
  默认连接
  redis-cli
  
  连接指定服务器
  redis-cli -h 127.0.0.1
  redis-cli -p 6379
  redis-cli -h 127.0.0.1 -p 6379
```
- **持久化**  
1. 利用永久性存储介质将数据保存，在特定时间将保存的数据进行恢复  
2. 持久化用于防止数据的意外丢失，确保数据安全性  
3. 数据状态持久化：快照形式，存储数据，存储格式简单，关注点在数据；操作过程持久化：日志形式，存储操作过程，存储格式复杂，关注点在数据的操作过程  
![](./Pics/持久化.png)  
- **RDB**  
1. `save`指令  
```
  语法：save
  
  作用：手动执行一次保存操作
  
  save指令相关配置： 
  dbfilename dump.rdb
    设置本地数据库文件名，默认值为dump.rdb
    通常设置为dump-端口号.rdb
  dir
    设置存储.rdb文件的路径
  rdbcompression yes
    设置存储至本地数据库时是否压缩数据，默认为yes
    通常默认为开启状态，如果设置为no，可以节省CPU运行时间，但会使存储的文件变大
  rdbchecksum yes
    设置是否进行RDB文件格式校验，该校验过程在写文件和读文件过程中均进行
    通常默认为开启状态，如果设置为no，可以节约读写性过程约10%时间消耗，但是有一定的数据损坏的风险
    
  cat redis-6379.conf
  # port 6379
  # daemonize yes
  # logfile "6379.log"
  # dir /root/redis-5.0.5/data
  # dbfilename dump-6379.rdb
  # rdbcompression yes
  # rdbchecksum yes
```  
2. 数据量过大，单线程执行方式造成效率过低：`save`指令的执行会阻塞当前`Redis`服务器，直到当前`RDB`过程完成为止，有可能造成长时间阻塞，线上环境不建议使用  
3. `bgsave`指令  
![](./Pics/Bgsave.png)  
```
  语法：bgsave
  
  作用：手动启动后台保存操作，但不是立即执行
  
  bgsave指令相关配置：
  dbfilename dump.rdb
  dir
  rdbcompression yes
  rdbchecksum yes
  stop-writes-on-bgsave-error yes
    后台存储过程中如果出现错误，是否停止保存操作
    通常默认为开启状态
```
4. `bgsave`命令是针对`save`阻塞问题做的优化，`Redis`内部涉及到`RDB`操作都采用`bgsave`方式  
5. `save`配置
![](./Pics/Save配置原理.png)
```
  配置：save seconds changes
  
  作用：满足限定时间范围内key的变化数量达到指定数量则进行持久化
  
  参数：
    seconds：监控时间范围
    changes：监控key的变化量
    
  cat redis-6379.conf
  # port 6379
  # daemonize yes
  # logfile "6379.log"
  # dir /root/redis-5.0.5/data
  # dbfilename dump-6379.rdb
  # rdbcompression yes
  # rdbchecksum yes
  # save 10 2
```  
6. `save`配置启动后执行的是`bgsave`操作；`save`配置要根据实际业务情况进行设置，频度过高或过低都会出现性能问题  
7. `RDB`启动方式对比  
![](./Pics/RDB启动方式对比.png)  
8. `RDB`特殊启动方式  
```
  全量复制
  
  服务器运行过程中重启
  debug reload
  
  关闭服务器时指定保存数据
  shutdown save
```  
9. `RDB`优点：   
a. `RDB`采用紧凑压缩的二进制文件，存储效率较高  
b. `RDB`内部存储`redis`在某个时间点的数据快照，适合于数据备份，全量复制等场景  
c. `RDB`恢复数据的速度比`AOF`快很多  
d. 应用：服务器定期执行`bgsave`备份，并将`RDB`文件拷贝到远程机器中，用于灾难恢复  
10. `RDB`缺点：  
a. `RDB`方式无论是执行指令还是利用配置，都无法做到实时持久化，具有较大的可能性丢失数据  
b. `bgsave`指令每次运行需要执行`fork`操作创建子进程，性能会有所降低  
c. `Redis`众多版本中未进行`RDB`文件格式的版本统一，可能出现各版本服务器之间的数据格式无法兼容的现象  
- **AOF**
1. `AOF`持久化：以独立日志的方式记录每次写命令，重启时重新执行`AOF`文件中命令以达到恢复数据的目的，与`RDB`相比可以简单描述为[由记录数据转变为记录数据产生的过程]  
2. `AOF`的主要解决数据持久化的实时性问题，目前已经称为`Redis`持久化的主流方式  
![](./Pics/AOF写数据过程.png)  
3. `AOF`写数据的三种策略（`appendfsync`）  
```
  always（每次）
    每次写入操作均同步到AOF文件中，数据零误差，性能较低，不建议使用
  everysec（每秒）
    每秒将缓冲区中的指令同步到AOF文件中，数据准确性较高，性能较高，建议使用，也是默认配置
    在系统突然宕机的情况下会丢失1秒内的数据
  no（系统控制）
    由操作系统控制每次同步到AOF文件的周期，整体过程不可控
```  
4. `AOF`相关配置  
```
  配置：appendonly yes|no
  作用：是否开启AOF持久化功能，默认为不开启状态
  
  配置：appendfsync always|everysec|no
  作用：AOF写数据策略
  
  配置：appendfilename filename
  作用：AOF持久化文件名，默认文件名为appendonly.aof，建议配置为appendonly-端口号.aof
  
  配置：dir
  作用：AOF持久化文件保存路径，与RDB持久化文件保持一致即可
```  
5. `AOF`重写  
![](./Pics/AOF写数据瓶颈.png)  
a. `Redis`引入`AOF`重写机制压缩文件体积，`AOF`文件重写是将`Redis`进程内的数据转化为写命令同步到新的`AOF`文件的过程，简单说，是对同一个数据的若干条命令执行结果，转化为最终结果数据所对应的指令，进行记录  
b. 作用：降低磁盘占用量，提高磁盘利用率；提高持久化效率，提高数恢复效率  
c. 重写方式  
```
  手动重写
  bgrewriteaof
  
  自动重写
  auto-aof-rewrite-min-size size
  auto-aof-rewrite-percentage percentage
  
  自动重写触发比对参数（运行指令info persistence获取具体信息）
  aof_current_size
  aof_base_size
  
  自动重写触发条件
  aof_current_size > auto-aof-rewrite-min-size
  (aof_current_size - aof_base_size) / aof_base_size >= auto-aof-rewrite-percentage
```  
![](./Pics/AOF手动重写_bgrewriteaof原理.png)  
d. 重写流程  
![](./Pics/AOF重写流程1.png)  
![](./Pics/AOF重写流程2.png)  
- **RDB和AOF对比**  
![](./Pics/RDB和AOF对比.png)  
```
  对数据非常敏感，建议使用默认的AOF持久化方案；数据呈现阶段有效性，建议使用RDB持久化方案
    若不能承受数分钟以内的数据丢失，选用AOF
    若能承受分钟以内的数据丢失，且追求大数据集的恢复速度，选用RDB
    灾难恢复选用RDB
    双保险策略，同时开启RDB和AOF，重启后，Redis优先使用AOF恢复数据，减少数据的丢失
```
