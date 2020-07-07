
#### Redis 数据结构指令

###### String字符串

- 格式 set key val，一个键最大能存储512M
- 格式 get key
- 格式 strlen key 长度

###### Hash  哈希表

- 格式 hset name key val
- 格式  hget name key
- 格式  hmset name  key1 val1 key2 val2..... 设置多个
- 格式 hmget name key1 key2
- 格式 hmgetall name 
- 格式 hkeys name  返回哈希表中所有key
- 格式 hlen name  返回哈希表的字段长度

###### List 列表

- 格式 lpush(rpush) key val1...
- 格式 lpop(rpop) key
- 格式 llen key 
- 格式 lrange key start end  取区间内元素

###### Set 集合

- 格式 sadd key val1,val2..... 
- 格式 spop key 随机移除返回一个数
- 格式 scard key  返回集合中元素数量
- 格式 smembers key 返回集合中的元素

###### ZSet 有序集合

- 格式 zadd key score1 val1 score2 val 2   
- 格式 zcard key 返回有序集合的成员数
- 格式 zcount key min max  计算有序集合中指定区间分数的成员数
- 格式 zrange key start end  显示指定区间的有序成员列表
- 格式 zrank key  val, 返回指定val值的排名，有序集合按照分值升序
- 格式 zrem key val1，val2.... 移除指定val
- 格式 zscore key val  返回有序集合，成员的分数值













[**Redis 基础数据结构和操作API**](https://www.imooc.com/article/31965)

[**Redis数据接口API介绍**](https://blog.csdn.net/weixin_42739916/article/details/88089193)
