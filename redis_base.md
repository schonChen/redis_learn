## redis 适用场景
- 取最新N个数据的操作
- 排行榜应用,取TOP N 操作
- 需要精确设定过期时间的应用
- 计数器应用
- Uniq操作,获取某段时间所有数据排重值
- 实时系统,反垃圾系统7.Pub/Sub构建实时消息系统
- Pub/Sub构建实时消息系统8.构建队列系统
- 缓存
  > redis 做为缓存，同时也会将数据写到硬盘上。所以数据是安全的(除突然断电外,重启服务会写到dump.rdb文件中)
## redis 基本操作
- 查看是否启动成功 ps -ef | grep redis
- redis-cli#进入客户端 quit/ctrl-c 退出
- 关闭redis pkill redis-server
- redis-cli shutdown#关闭
## redis 数据类型及操作
### 5种数据类型:字符串、哈希、链表、集合、有序集合。
  > 支持:push/pop、add/remove 、取交集、并集、差集、排序。
  > redis<===同步====>mysql
  > 同时也会将数据写到硬盘上。所以数据是安全的(除突然断电外,重启服务会写到dump.rdb文件中)
- select num#选择库,默认在0库,共16个库
- auth meili.chen#授权用户所需密码(密码就是redis.conf中配置的密码)
- flushdb#清空数据库。
### String(字符串)类型: 
- set name lijie#设置键name的值为lijie
- get name#获取name的值。
- keys *#查询所有的键。
- setnx name liweijie#如果键已存在则返回0,不更新,防止覆盖。
- setex haircolor 10 red #设置键的值的有效期为10秒。
- setrange email 6 lampbre.com#替换键的值从第6个字符开始换为lampbre.com
- mset name1 李大伟 name2 李小伟#设置多个键的值。
- msetnxname1 张三 name3 李四#判断键是否存在,不存在则设置,否则不设置返回0
- mget name1 name2 name3#一次获取多个键的值。
- getset name1 Tom#重新设置键的值,并返回旧的键值。
- getrange email 6 18#截取email键的值,从第6-18位间的字符。
- incr uid#每次自增1 (如果key中uid不存在,则设置并从0开始,下同)
- incrby uid 5#每次自增5 
- incrby uid -5#每次自减5 
- decr uid #每次自减1
- decrby uid 5#每次自减5
- appendname1 @126.com#给name1的值,添加字符串@126.com
- strlenname1#返回键name1的值的长度。
### Hashes(哈希)类型: 
- hset user:001 name liweijie#哈希设置用户user:001的name键值为liweijie
- hset user:001 age 21#同样,增加一个age键值为21
- hsetnx user:001 age 22#同上,但检测键是否存在。若不存在创建。
- hmset user:002 name liweijie2 age 26 sex 1#同时设置多个键的值。
- hget user:001 name#哈希获取用户user:001的name键的值。
- hget user:001 age #同上。
- hmget user:001 name age sex#获取多个指定的键的值。
- hgetall user:001#获取所有键的值。
- hincrbyuser:001 age -8#在指定键上加上给定的值。
- hexists user:001 sex#检测指定的键值是否存在。
- hlen user:001#返回指定哈希的键个数/字段个数。
- hdel user:001 sex#删除指定(user:001)哈希的指定字段或是键值。
- hkeys user:003#返回哈希里所有字段或是键值。
### Lists(链表)类型及操作(棧或队列): 
- lpush mylist "world"#从头部插入字符串
- lpush mylist "hello"#同上
- lrange mylist 0 -1#获取从0到最后一个如[1) "hello" 2) "world"]
- rpush mylist "jiejie"#在尾部插入
- linsert mylist before "hello" "this is linsert" #指定插入位置(在hello之前插入)。
- lset mylist 0 "what"#设置修改指定下标的值。
- lrem mylist 1 "hello"#删除(1个)一个值为hello的元素。(n<0从尾部删除,n=0全部删除)
- ltrim mylist 1 2 #保留表中下标为1/2的元素。
- lpop mylist#弹出开头元素并返回。
- rpop mylist#弹出尾部元素并返回。
- rpoplpush mylist mylist2 #从mylist尾部弹出插入到mylist2的头部。
- lindex mylist 0#获取表下标为0的元素值。
- llen mylist#返回表元素个数(相当于count($arr  ))。
### sets(集合)类型及操作(好友推荐、blog、tag功能): 
- smembers myset#查看myset集合中所有元素值。
- sadd myset "hello"#向mysets集合中添加一个值hello
- srem myset "hello"#删除myset集合中名称为hello的元素。
- spop myset #随机弹出并返回mysets中的一个元素。
- sdiff myset2 myset3#返回myset2中的与myset3的差集(以myset2为准)。
- sdiffstore myset4 myset2 myset3#返回myset2中的与myset3的差集,并存入myset4中去。
- sinter myset2 myset3#返回myset2与myset3的交集。
- sinterstore myset5 myset2 myset3#返回myset2与myset3的交集,并存入myset5中去。
- sunion myset2 myset3#求并集(去重复)
- sunionstore myset6 myset2 myset3#求并集,并存入myset6中去。
- smove myset2 myset3 "three"#将myset2中的three移到myset3中去。
- scard myset2#返回元素个数。
- sismember myset2 "one"#判断元素one是不是myset2集合的(相当于is_array())。
- srandmember myset2#随机返回myset2集合中的一个元素,但不删除(相当于array_rand())。
### sorted sets(有序集合)类型及操作(以scores排序): 
- zadd myzset 1 "one"#向顺序1的添加元素one
- zadd myzset 2 "two"#同上。
- zadd myzset 3 "two"#相当于更新顺序为2的值
- zrange myzset 0 -1 withscores#查看所有元素并带上排序(默认升序)。
- zrem myzset "two"#删除two
- zincrby myzset 2 "two"#将two的顺序值加上2
- zrank myzset "two"#返回集合中元素的索引下标值。
- zrevrank myzset two#元素反转并返回新下标值。
- zrevrange myzset 0 -1 withscores#按顺序反转(相当于降序排序)
- zrangebyscore myzset 1 10 withscores#返回顺序为1-10的元素(可做分页)。
- zcount myzset 1 10 #返回顺序在1-10之间元素的个数。
- zcard myzset#返回集合中所有元素的个数。
- zremrangebyrank myzset 1 2#删除集合中下标为1到2的元素。
- zremrangebyscore myzset 1 10#删除集合中顺序为1到10的元素。
### Redis常用命令  
- 键/值相关命令。
- keys * #查询所有
- keys user*#查询指定的
- exists user:001#判断是否存在。
- del name#删除指定的键。
- expire addr 10#设置过期时间
- ttl addr#查询过期时间
- select 0 #选择数据库
- move age 1#将age移到1数据库。
- get age #获取
- persist age#移除age的过期时间。
- randomkey#随机返回一个key
- rename name1 name2#重命名键
- type myset#返回键的类型。
- ping #测试redis连接是否存活。
- echo lamp#输出一个lamp
- select 10#选择数据库。
- quit/exit/crtl+C#退出客户端
- dbsize#返回库里的键的个数。
- 服务器相关命令:
- info#显示redis服务器的相关信息。
- config get */loglevel #返回所有/指定的配置信息。
- flushdb#删除当前库中的所有键/表。
- flushall#删除所有数据库中的所有键/表

