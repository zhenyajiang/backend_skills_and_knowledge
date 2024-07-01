# 事务隔离级别
### 1.一句话描述脏读，不可重复读，幻读

脏读：T1读取到T2未提交的数据。

不可重复读：T1多次执行Query1，读取到的rows相同，values不同（被T2 update了）。

幻读：T1多次执行Query1， 读取到的rows不同（被T2 insert或者delete了）。

### 2.一句话描述MVCC
提供一种读取快照机制，一定程度上解决了读写冲突的问题。只在Repeatable READ和Read Commited隔离级别有效。

### 3.一句话描述Next-Key Locking解决了什么问题？

<b>一定程度上解决了Repeatable READ隔离级别下的幻读问题。</b>

几种反例：

1.查询没有使用索引

2.查询包含复杂的join操作

### 4.Next-Key Locking和Record Lock, Gap Lock

Next-Key Locking包含了Record Locks和Gap Locks。是两者的结合。

### 5.MySQL检查死锁命令

show process list

可以检查活跃的数据库连接和他们正在进行的操作。通过观察 State 列，你可以看到进程是否在等待某种资源，例如锁。如果多个进程在等待资源，而这些资源又分别被其他进程占用，就可能形成死锁。
例如，如果一个进程处于 "Waiting for table metadata lock" 或 "Lock wait" 状态，这可能表明存在锁竞争。如果这样的状态持续很长时间，或者你发现多个进程相互等待对方释放资源，这可能是死锁的迹象。

SHOW ENGINE INNODB STATUS

深入检查死锁可使用这个命令。这个命令可以显示SQL语句。

SELECT * FROM information_schema.innodb_locks; 和 SELECT * FROM information_schema.innodb_lock_waiters;


这些命令分别显示当前的锁和等待锁的信息，帮助你确定哪些事务持有锁，哪些事务在等待。