# Redis的数据结构
## 基础数据结构
### SDS 简单动态字符串
搞清楚int, raw , embstr的区别

### quicklist 快速链表
### ziplist 压缩链表
### skiplist 跳表
### hashtable 哈希表
### intset 整数集合
目的是节省set的存储空间

## 数据结构
### string
底层使用SDS，SDS的底层是int, raw, embstr。

### set
底层使用hashtable， 如果set中元素较少(默认是小于512），且都是数字，则使用intset。

### list
老版本使用ziplist和linkedlist。3.2开始引入quicklist。5.0引入listpack。

### zset
底层使用skiplist和hashtable组合实现。在数据量较小的情况下使用ziplist和hashtable。

### hash
底层使用ziplist或hashtable实现。在数据量较小（数量小于512，且所有值的key和value长度都不超过64）的情况下使用ziplist。否则使用hashtable。