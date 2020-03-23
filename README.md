# Redis存储技术

## 一、Redis介绍

### 1、Redis简介

Redis是一个开源项目，遵循BSD协议，是一个高性能的key-value数据库。

### 2、Redis特点

- 支持数据的持久化
- 支持list、set等数据结构存储
- 支持数据备份

### 3、Redis优势

- 性能极高；读11万/s，写8.1万/s
- 丰富的数据类型：String、hash、list、set、zset
- 操作具有原子性
- 丰富的特性：支持publish/subscribe，通知，key过期等等特性

## 二、Redis安装

下载地址：https://github.com/microsoftarchive/redis/releases

## 三、Redis配置

### 1、配置文件修改

直接**【修改】**安装目录根目录redis.conf（Windows 名为 redis.windows.conf）

### 2、命令修改

- 查看语法（这里CONFIG_SETTING_NAME可以是单个配置名，也可以用*获取所有）

```properties
127.0.0.1:6379> CONFIG GET CONFIG_SETTING_NAME
```

- 查看实例

```properties
127.0.0.1:6379> CONFIG GET loglevel
1) "loglevel"
2) "notice"
```

- 编辑语法

```properties
127.0.0.1:6379> CONFIG SET CONFIG_SETTING_NAME NEW_CONFIG_VALUE
```

- 编辑实例

```properties
127.0.0.1:6379> CONFIG SET loglevel "notice"
OK
```

### 3、配置参数说明

查看https://www.runoob.com/redis/redis-conf.html说明

## 四、Redis数据类型

- String（字符串）
- Hash（哈希）
- List（列表）
- Set（集合）
- Zset（有序集合）

## 五、Redis命令

- 服务端启动

```properties
F:\software\redis>redis-server.exe
```

- 客户端启动

  - 本地执行启动

  ```properties
  F:\software\redis>redis-cli
  ```

  - 远程服务执行

  ```properties
  redis-cli -h host -p port -a password
  ```

- 检查是否启动服务端

```properties
127.0.0.1:6379> ping
PONG
```

- 帮助信息查看

```properties
To get help about Redis commands type:
      "help @<group>" to get a list of commands in <group>
      "help <command>" for help on <command>
      "help <tab>" to get a list of possible help topics
      "quit" to exit
```

## 六、Redis键（key）

### 1、语法

```properties
127.0.0.1:6379> COMMAND KEY_NAME
```

### 2、实例

```properties
127.0.0.1:6379> SET mykey 123
OK
127.0.0.1:6379> GET mykey
"123"
```

**以上案例是GET是一个命令，mykey是一个key是一个键**

### 3、keys命令

更多命令查看https://www.runoob.com/redis/redis-keys.html

## 七、Redis字符串（String）

### 1、语法

```properties
127.0.0.1:6379> COMMAND KEY_NAME
```

### 2、实例

```properties
127.0.0.1:6379> set mykey 123
OK
127.0.0.1:6379> get mykey
"123"
```

以上使用了**SET**和**GET**命令，键名为mykey

### 3、字符串命令

更多命令查看https://www.runoob.com/redis/redis-strings.html

## 八、Redis哈希（Hash）

### 1、实例

```properties
127.0.0.1:6379> HMSET mykey name "zhangsan" age 10
OK
127.0.0.1:6379> HGETALL mykey
1) "name"
2) "zhangsan"
3) "age"
4) "10"
```

实例中，我们设置（name，age）的信息到哈希表mykey中

### 2、Hash命令

更多命令查看https://www.runoob.com/redis/redis-hashes.html

## 九、Redis列表（List）

### 1、实例

- 多个值一次性添加

```properties
127.0.0.1:6379> lpush mykey mysql oracle informix
(integer) 3
127.0.0.1:6379> lrange mykey 0 3
1) "informix"
2) "oracle"
3) "mysql"
```

- 单个值多次添加

```properties
127.0.0.1:6379> lpush mykey mysql
(integer) 1
127.0.0.1:6379> lpush mykey oracle
(integer) 2
127.0.0.1:6379> lpush mykey informix
(integer) 3
127.0.0.1:6379> lrange mykey 0 100
1) "informix"
2) "oracle"
3) "mysql"
```

以上案例使用**lpush**将值插入名为mykey的列表中

### 2、列表命令

更多命令查看https://www.runoob.com/redis/redis-lists.html

## 十、Redis集合（Set）

### 1、实例

- 单个值多次添加

```properties
127.0.0.1:6379> sadd mykey redis
(integer) 1
127.0.0.1:6379> sadd mykey mongodb
(integer) 1
127.0.0.1:6379> smembers mykey
1) "redis"
2) "mongodb"
```

- 多个值一次性添加

```properties
127.0.0.1:6379> sadd mykey redis mongodb
(integer) 2
127.0.0.1:6379> smembers mykey
1) "redis"
2) "mongodb"
```

### 2、集合命令

更多命令查看https://www.runoob.com/redis/redis-sets.html

## 十一、Redis有序集合（Sorted set）

### 1、实例

```properties
127.0.0.1:6379> zadd mykey 1 redis
(integer) 1
127.0.0.1:6379> zadd mykey 2 mongodb
(integer) 1
127.0.0.1:6379> zrange mykey 0 10
1) "redis"
2) "mongodb"
127.0.0.1:6379> zrange mykey 0 10 withscores
1) "redis"
2) "1"
3) "mongodb"
4) "2"
```

### 2、有序集合命令

更多命令查看https://www.runoob.com/redis/redis-sorted-sets.html

## 十二、Redis HyperLogLog

### 1、介绍

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基 数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

### 2、什么是基数

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。 基数估计就是在误差可接受的范围内，快速计算基数。

### 3、实例

```properties
127.0.0.1:6379> pfadd mykey redis mysql mongodb
(integer) 1
127.0.0.1:6379> pfcount mykey
(integer) 3
127.0.0.1:6379> pfadd mykey2 java
(integer) 1
127.0.0.1:6379> pfmerge mykey3 mykey mykey2
OK
127.0.0.1:6379> pfcount mykey3
(integer) 4
```

### 4、命令

更多命令查看https://www.runoob.com/redis/redis-hyperloglog.html

## 十三、Redis发布订阅

### 1、介绍

Redis 发布订阅(pub/sub)是一种消息通信模式：发送者(pub)发送消息，订阅者(sub)接收消息。

Redis 客户端可以订阅任意数量的频道。

![](images/1.jpg)

### 2、实例

```properties
#一个redis客户端，建立频道
127.0.0.1:6379> subscribe mychannel
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "mychannel"
3) (integer) 1

#启动另一个redis客户端，发布信息
127.0.0.1:6379> publish mychannel "hello"
(integer) 1
127.0.0.1:6379> publish mychannel "world"
(integer) 1

#在建立频道的客户端输出
1) "message"
2) "mychannel"
3) "hello"
1) "message"
2) "mychannel"
3) "world"
```

### 3、命令

更多命令查看https://www.runoob.com/redis/redis-pub-sub.html

## 十四、Redis事务

### 1、介绍

Redis 事务可以一次执行多个命令， 并且带有以下三个重要的保证：

- 批量操作在发送 EXEC 命令前被放入队列缓存。
- 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
- 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

一个事务从开始到执行会经历以下三个阶段：

- 开始事务。
- 命令入队。
- 执行事务。

### 2、实例

这里使用multi开始事务，然后输入多个命令，最后由 **EXEC** 命令触发事务， 一并执行事务中的所有命令：

```properties
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set book-name "c++"
QUEUED
127.0.0.1:6379> get book-name
QUEUED
127.0.0.1:6379> sadd tag "c++" "programming"
QUEUED
127.0.0.1:6379> smembers tag
QUEUED
127.0.0.1:6379> exec
1) OK
2) "c++"
3) (integer) 2
4) 1) "c++"
   2) "programming"
```

单个 Redis 命令的执行是原子性的，但 Redis 没有在事务上增加任何维持原子性的机制，所以 Redis 事务的执行并不是原子性的。

事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做指令的回滚，也不会造成后续的指令不做。

### 3、命令

更多命令查看https://www.runoob.com/redis/redis-transactions.html

## 十五、Redis脚本（Lua脚本）

### 1、语法

```properties
127.0.0.1:6379> eval script numkeys key [key ...] arg [arg ...]
```

### 2、实例

```properties
127.0.0.1:6379> eval "return {KEYS[1],KEYS[2],ARGV[1],ARGV[2]}" 2 key1 key2 first second
1) "key1"
2) "key2"
3) "first"
4) "second"
```

### 3、命令

更多命令查看https://www.runoob.com/redis/redis-scripting.html

## 十六、Redis连接

### 1、实例

```properties
127.0.0.1:6379> auth "password"
OK
127.0.0.1:6379> ping
PONG
```

如果密码未设置，可以使用config set requirepass password进行设置

### 2、命令

更多命令查看https://www.runoob.com/redis/redis-connection.html

## 十七、Redis服务器

### 1、实例

```properties
127.0.0.1:6379> info
# Server
redis_version:3.2.100
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:dd26f1f93c5130ee
redis_mode:standalone
os:Windows
arch_bits:64
multiplexing_api:WinSock_IOCP
process_id:11216
run_id:44be3684ff4d572a7b242d7070a03eb174f00b4e
tcp_port:6379
uptime_in_seconds:2083
uptime_in_days:0
hz:10
lru_clock:6083620
executable:F:\software\redis\redis-server.exe
config_file:

# Clients
...
```

### 2、命令

更多命令查看https://www.runoob.com/redis/redis-server.html

## 十八、Redis数据备份与恢复

### 1、实例

```properties
127.0.0.1:6379> save
OK
```

使用save命令进行备份，在 redis 安装目录中创建dump.rdb文件，可以通过config get dir获取redis安装目录

### 2、数据恢复

```properties
127.0.0.1:6379> bgsave
Background saving started
```

## 十九、Redis安全

### 1、实例

- 查看是否设置密码

```properties
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) ""
```

- 设置密码

```properties
127.0.0.1:6379> config set requirepass "redis"
OK
```

- 校验密码

```properties
127.0.0.1:6379> auth redis
OK
```

## 二十、Redis性能测试

### 1、语法

```properties
redis-benchmark [option] [option value]
```

**注意**：该命令是在 redis 的目录下执行的，而不是 redis 客户端的内部指令。

### 2、更多指令

https://www.runoob.com/redis/redis-benchmarks.html

## 二十一、Redis客户端连接

### 1、实例

```properties
127.0.0.1:6379> config get maxclients
1) "maxclients"
2) "10000"
```

### 2、命令

https://www.runoob.com/redis/redis-client-connection.html

## 二十二、Redis管道技术

- Redis 管道技术

Redis 管道技术可以在服务端未响应时，客户端可以继续向服务端发送请求，并最终一次性读取所有服务端的响应

- 管道技术的优势

管道技术最显著的优势是提高了 redis 服务的性能。

更多查看：https://www.runoob.com/redis/redis-pipelining.html

## 二十三、Redis分区

https://www.runoob.com/redis/redis-partitioning.html

## 二十四、Java使用Redis

### 1、具备java开发环境

### 2、下载redis驱动包（jedis.jar）

链接：https://mvnrepository.com/artifact/redis.clients/jedis

### 3、连接redis服务

```java
package com.redis;

import redis.clients.jedis.Jedis;

import java.util.Iterator;
import java.util.List;
import java.util.Set;

/**
 * @author sayyes
 * @date 2020/3/23
 */
public class TestDemo {
    public static void main(String[] args) {
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost");
        System.out.println("连接成功");
        //查看服务是否运行
        System.out.println("服务正在运行: " + jedis.ping());
        System.out.println("---------------------------");

        //设置 redis 字符串数据
        jedis.set("runoobkey", "www.runoob.com");
        // 获取存储的数据并输出
        System.out.println("redis 存储的字符串为: " + jedis.get("runoobkey"));
        System.out.println("---------------------------");

        //存储数据到列表中
        jedis.lpush("site-list", "Runoob");
        jedis.lpush("site-list", "Google");
        jedis.lpush("site-list", "Taobao");
        // 获取存储的数据并输出
        List<String> list = jedis.lrange("site-list", 0, 2);
        for (int i = 0; i < list.size(); i++) {
            System.out.println("列表项为: " + list.get(i));
        }
        System.out.println("---------------------------");

        // 获取数据并输出
        Set<String> keys = jedis.keys("*");
        Iterator<String> it = keys.iterator();
        while (it.hasNext()) {
            String key = it.next();
            System.out.println(key);
        }
    }
}

```

如果出现密码验证问题：

1、在程序中添加密码*.auth()

2、redis中config set requirepass “”关闭密码校验



