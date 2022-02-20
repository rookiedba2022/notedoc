> - 主要参考：
>
>   - [老男孩MySQL DBA 57期](https://space.bilibili.com/350243622/channel/seriesdetail?sid=1656810)
>
>   - [《深入浅出MySQL》第三版](https://book.douban.com/subject/34826424/)
>   - [《高性能MySQL(第3版)》](https://book.douban.com/subject/23008813/)
>   - [《MySQL必知必会》](https://book.douban.com/subject/3354490/)

## 一、数据库的介绍

### 1. 产品分类

- RDBMS
  - MySQL
  - Oracle(11G, 12C)
  - PG
  - MS SQL

> 建议：
>
> 除了MySQL，额外多掌握一款其他的数据库
>
> 学习路径：
>
> 1. 安装
> 2. 体系结构
> 3. 基础管理(用户 权限 网络 表空间 数据文件)
> 4. 备份恢复(expdb/impdp rman)
> 5. 高可用(RAC DG)

- NoSQL
  - Redis
  - MongoDB
  - ES
- NewSQL
  - TiDB
- 云数据库
  - RDS

### 2. MySQL数据库的分支和版本

- MySQL
  - Oracle MySQL
    - 5.6, 5.7, 8.0
  - Percona MySQL
  - MariaDB
  - RDS/TDB
- Oracle
  - 11g
    - 11.2.0.4
  - 12c
    - 12.2.x
  - 18c

## 二、MySQL的安装

### 1. 安装方式

- Linux
  - 二进制安装
  - 源码安装
    - 方便二次开发
  - yum安装
- windows
- mac

## 三、MySQL体系结构及基础管理

### 1. C/S工作模型

客户端(C)$\to$服务端(S)

<img src='/img/mysql_c_s.jpg'></img>

### 2. 实例

mysqld核心守护进程 + Master Thread 主线程 + 其他线程 + 预分配内存 = 实例

<img src='/img/mysql_instance.jpg'></img>

### 3. MySQL程序结构(mysqld)

- server层
  - 连接层+SQL层
- engine层
  - 插件式的存储引擎

SQL的基本的执行流程

1. Client
   - 提供连接API
2. 连接层
3. SQL层
4. 存储引擎层

<img src='/img/how_sql_runs.jpg'></img>

### 4. MySQL的逻辑结构

- 库
  - 库名+表属性
- 表
  - 表名+列+数据行+表属性

### 5. MySQL的物理结构

- 段
  - 一个表就是一个段
  - 分区表除外
- 区
  - 连续的64个Page
- 页
  - 16kb，连续的4个os block

### 6. 用户、权限管理

#### 6.1 用户管理

- 用户的作用
  - 登陆MySQL
  - 管理MySQL对象
- 定义
  - `用户名@'IP白名单'` 
    - 用户名
      - 必须是字符串
    - 白名单
      - localhost 
        - 只能由本地用户
      - 10.0.0.1 
        - 允许该IP
      - 10.0.0.%
        - 允许该网段10.0.0.0~10.0.0.254
      - 10.0.0.5%
        - 允许10.0.0.50~10.0.0.59
      - %
        - 允许所有IP
      - db01
        - 允许主机名为db01
- 管理
  - 查询
    - `select user, host, authentication_string from mysql.user; `
  - 创建
    - `create user 用户名@'白名单' identified by '密码';`
  - 删除
    - `drop user 用户名@'白名单';`
  - 修改(密码)
    - `alter user 用户名@'白名单' identified by '新密码';`

#### 6.2 权限管理

- 查看所有权限
  - `show privileges;`
- 授权语句
  - `grant 权限 on 权限范围 to 用户名@'白名单' [with grant option];`
    - 权限
      - ALL
        - 除了Grant optin以外的所有权限
      - 自己指定的权限
    - 权限范围
      - `*.*`
        - 全库级别
        - 管理员
      - `db.*`
        - db库下的所有表
        - 单库级别
        - 普通用户
      - `db.t1`
        - db库下的t1表
        - 单表级别
    - `with grant opion`
      - 有创建其他用户的权限
      - 可以不指定
- 用户授权管理
  - 回收权限
    - `revoke 权限 on 库.表 from 用户名@'白名单';`
  - 查看授权
    - `show grants for 用户@'白名单';`
- 权限对应的文件
  - 授权表
    - `mysql.user`
      - 存储用户信息+全库级别的权限
    - `mysql.db`
      - 单库级别的权限
    - `mysql.tables_priv`
      - 表级别的权限
    - `mysql.columns_priv`
      - 列级别的权限
    - `mysql.procs_priv`
    - `mysql.procies_priv`

### 7. 连接管理

1. 自带客户端程序
   - mysql命令
     - socket
       - 前提
         - 提前授权白名单为`localhost`的用户
       - 查看socket位置
         - `select @@socket;`
       - 连接
         - `mysql -uroot -p123 -S /tmp/tmp.sock`
     - TCP/IP
       - 前提
         - 提前授权远程连接的用户
       - 连接
         - `mysql -uroot -p123 -h 192.168.1.105 -P 3306`
2. 客户端工具
   - mysql workbench (建议)
   - sqlyog
   - navicat

> 不建议在公司里用破解版，因为可能会有恶意软件

3. 程序API连接
   - php
     - pho-mysql
   - Python
     - pyMysql

### 8. MySQL的启动和关闭

- 启动
  - 自带脚本
    - `support-files/mysql.server`
  - mysqld
  - mysqld_safe

### 9. 配置文件

- 默认读取路径
  - 查询
    - ``
- 格式
  - 标签

## 四、SQL语句

### 1. SQL_MODE

- `Only_full_group_by`
  - 5.7版本后新加入的，select后的列，如果没有在group by或者没有在函数中

### 2. 字符集（utf8与utf8mb4区别）

### 3. 数据类型

- char和varchar区别

### 4. DDL规范

- ONLINE DDL
  - pt-osc
  - gh-ost

> 业务繁忙期间不要做大表的DDL操作

- 建表规范

### 5. delete truncate drop table区别

### 6. select的查询逻辑

- 单表select的执行顺序
  1. from
  2. where
  3. group by
  4. select_list
  5. having
  6. order by
  7. limit

- 多表
  1. join
  2. on
  3. where
  4. group by
  5. left join

### 7. information_schema.tables / columns

做一些数据库资源的统计

## 五、索引及执行计划管理

### 1. MySQL的索引类型

- 类型
  - Btree (Blance Tree)
  - InnoDB，MyISAM
- 哈希索引
  - Memory
  - InnoDB也会维护自己的AHI的hash索引
- FULLTEXT
  - 全文索引
  - 一般在大字段使用，用ES数据库存储大字段
- GIS
  - 地理位置索引
  - 一般MongoDB可以替代

### 2. MySQL BTree索引的构建逻辑

#### MySQL为什么要用BTree(B+ Tree)算法

- 相比其他的数据结构（二叉树、红黑树......），BTree是**平衡**的
  - 不管找任何一个值，都要找3次
- MySQL中的BTree，至少有一个根和页，枝是否有要看数据量

#### 

- 聚簇索引
  - 解决了`select * from t1 where id =10`
- 辅助索引
  - 解决了`select * from t1 where`
- 回表
  - 辅助索引查询完之后，再拿着查到的ID，去聚簇索引里找
  - 带来了大量的随机IO
- 怎么减少随机IO
  - 减少回表次数
    - 使用联合索引
      -
    - MRR
      -

### 3. MySQL索引树高度影响因素



### 4. 执行计划

- explain/desc
- type
  - 表现是否用了索引，以及索引的级别
- key_len
  - 联合索引是否合理
- extra
  - filesort

## 六、存储引擎

### 1. 种类

- InnoDB
- MyISAM
- Memory
- CSV
- Blackhole
- 第三方
  - ToukuDB
  - Myrocks

### 2. 操作

- 查看
- 更改
  - `alter table t1 engine=innodb;`
    - 作用
      - 改变存储引擎
      - 清理表碎片
- HWM(高水位)
  - 全盘扫描与范围查询
  -

### 3. InnoDB核心特性

- 事务 (ACID)
- MVCC
- 聚簇索引
- 行级锁
- 外键
- GTID
- 自适应哈希索引AHI
  - 将频繁访问的索引页，建立内存HASH表，达到快速访问这些索引的目的
- 热备份
- ACSR 自动故障恢复
- Insert buffer
  - 插入数据不会立即更新到索引树中，存储在change buffer里
- double write（二次写）

### 4. 事务

- ACID
- MVCC
- 锁
- 隔离级别

## 七、日志管理

### 1. 错误日志

### 2. 二进制日志

- 如何查看
  -
- 如何截取
  - position
  - gtid
    - skip-gtids
- `binlog2sql`
  - 根据binlog（要求格式为row）反解析成delete，update操作，对误删除的数据进行还原
- `binlog_format` （针对insert update delete操作）
  - SBR
  - RBR
  - MBR

### 3. 慢日志

- 如何开启
- 如何分析
  - `mysqldumpslow`
  - `pt-query-digest`
- 如何处理
  - kill操作无法快速kill
    - pt-kill

## 八、备份恢复

### 1. 职责

- 设计备份策略

  1. 备份工具

     - 100G以内
       - 逻辑备份、物理备份均可
     - 100G~1TB级别
       - 物理备份
     - 超大数据

  2. 备份周期

     - 数据量小，可以每天全备
     - 数据量大，可以采用每周或每月全备+增量+binlog

  3. 备份检查

     - 成功与否
       - 日志
       - 备份大小
       - 备份集情况

  4. 备份恢复演练

  5. 数据快速恢复

     - 常规
       - 全备+增量+binlog
     - 灵活
       - 物理损坏
         - 主从/高可用
       - 逻辑损坏
         - 删库
           -
         - 删表
         - 误操作数据行

  6. 数据库的迁移升级

     - 不要直接操作

     - 5.7升级到8.0无法回退

### 2. 备份工具使用

- mysqldump
  - 参数
    - `--master-data=2`
  - mysqldump全库备份加锁
- xtrabackup
  - 原理
- mydumper
- into outfile / load data infile

## 九、主从复制

### 1. 主从复制的前提（搭建过程）

1. 两台以上的数据库实例，需要不同的server_id，不同的server_uuid
2. 主库需要开启二进制日志，创建专用的复制用户
3. 备份主库数据，恢复到从库
4. 从库执行`change master to`
5. 从库`start slave;`

### 2. 主从复制的原理

#### 2.1 涉及到的文件与线程

##### 文件

- 主库
  - `binlog`
- 从库
  - `relay-log`
    - 中继日志
    - 存储请求过来的`binlog`
  - `matser.info`
    - 保存主库信息
    - IP, PORT ,USER, PASSWORD, binlog
  - `relay-log.info`
    - 记录从库回放`relay-log`的位置点

##### 线程

- 主库
  - `dump_thread`
    - 日志投递线程
- 从库
  - `IO`
    - 连接主库，请求日志
  - `SQL`
    - 回放日志

#### 2.2 原理

1. 从库执行`Change Master`语句，将主库相关信息(IP, PORT ,USER, PASSWORD, binlog及起点)写入到`master.info`文件中
2. 从库执行`start slave;`，启动从库中的`IO`线程、`SQL`线程
3. 从库`IO`线程读取`master.info`中的信息，连接主库
4. 主库连接层接收到请求，验证通过后，生成专用连接线程`dump`与`IO`线程交互
5. 从库`IO`线程，通过`master.info`中的binlog起点位置，向主库的`dump`线程请求`binlog`
6. 主库`dump`线程实时监控`binlog`变化，接收到从库`IO`请求，截取最新的`binlog`，并传输给`IO`
7. 从库`IO`接收到最新的`binlog`日志，先临时存储在`TCP/IP`缓存，主库工作到此为止
8. 从库`IO`将接收到的日志存储到`relay-log`中，并更新`master.info`中的起点
9. 从库`SQL`线程读取`relay.info`，获取到上次回放道已经执行到的位置点
10. 从库`SQL`回放新的`relay-log`，再次更新`relay.info`，`SQL`线程工作结束



1. `relay_log_purge`线程，对`relay-log`有自动清理的功能
2. 主库`dump`线程实时监控`binlog`的变化，自动通知给从库`IO`（并没有传输）

### 3. 主从复制的监控

#### 3.1 主库监控

主库监控较简单

#### 3.2 从库监控

```mysql
mysql> show slave status\G
*************************** 1. row ***************************
			   主库信息汇总
               Slave_IO_State: Waiting for master to send event
                  Master_Host: mysqldb_master
                  Master_User: repl
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000001
          Read_Master_Log_Pos: 1570
          
          
               从库relaylog回放到的位置点(记录在relay-log.info中)
               Relay_Log_File: mysqldb_slave-relay-bin.000002
                Relay_Log_Pos: 1783
        Relay_Master_Log_File: mysql-bin.000001
        
        
        	 从库的线程状态（记录在log_error中）
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
            
            
              过滤复制相关内容
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
  
  
        监控主从延时       
        Seconds_Behind_Master: 0
 
 
           GTID复制   
           Retrieved_Gtid_Set: 
            Executed_Gtid_Set: 
            
1 row in set (0.00 sec)
```

### 4. 主从复制故障

### 5. 主从延时

- 主库原因
- 从库原因

### 6. 特殊从库

- 延时从库
- 过滤复制
- 半同步
  - 原理
    1. 主库IO线程，将接收到的binlog写入relay-log中后，返回ACK确认
    2. 主库ACK rec线程，接收到ACK确认之后，主库库事务才commit成功
    3. 如果主库10s还没有接收到ACK确认，此次复制会被切换为异步复制

### 7. MGR

- 原理
  - paxos协议（分布式一致协议）

### 8. GTID复制

## 十、MHA高可用架构

### 1. 设计高可用架构

#### 1.1 主从复制架构演变

##### 传统复制

- 基础架构(仅用MySQL本身就能实现，不需要第三方软件的配合)
  - 一主一从
  - **一主多从**
  - 双主结构（互为主从）
  - **多级主从**

##### 只有主从复制是不够的

假如你构建了一个一主多从，你的应用只在master库进行读写，有一天master库突然挂了，应该怎么办？

很明显，是将应用连接到从库上去，但是这件事怎么去做呢？人为去做？还是自动转移？

一个常见的思路用keepalive做vip地址漂移，但是，主库从库之间并不是100%同步的，keepalive实现vip地址漂移只能保证实现切换到从库(Fail Over)，但是并不能保证切换到的从库数据一定与原来的主库是同步的，这并不是高可用。

高可用，不仅要保证**故障转移**，而且要保证从库、主库的**数据一致**。常见的高可用软件，有MHA、PXC、MGR等等。

但是MHA也是有自己的缺陷的，MHA无法实现读写分离，需要与其他的中间件一起配合使用，将读的压力分散到从库上。

以上所述的架构（一主多从+读写分离+keepalive+MHA）算是一定意义上的高可用，但它也有明显的缺陷：写的压力过大，这需要通过分布式数据库解决。

#### 1.2 高级架构

- 高可用
  - MHA
    - facebook
    - RDS MySQL (TMHA)
      - 两节点
    - Oracle官方的operator (K8S + MHA)
      - 实现自愈
  - PXC
  - MGC
  - MySQL Cluster
  - InnoDB Cluster
    - MySQL 8.0 普及后未来可期
- 读写分离
  - Atlas
  - ProxySQL
  - Maxscale
  - Mycat
- 分布式架构
  - Mycat
  - DBLE
  - sharding-JDBC

#### 1.3 MHA高可用搭建

##### 准备环境

一主二从GTID + 一监控

### 2. MHA故障切换的原理

### 3. MHA故障处理

### 4. 配合读写分离

- ProxySQL
- Atlas
- maxscale

### 5. 其他的高可用方案

- PXC
- MGC
- InnoDB Cluster

## 十一、分布式架构

- Mycat
- DBLE
- sharding-jdbc

## 十二、MySQL优化

- 优化思路（从下往上）
  1. 硬件
  2. 操作系统
  3. 文件系统
  4. MySQL实例
     - 参数
  5. 逻辑结构
     - schema设计
       - 库表设计规范
       - DDL操作
  6. SQL，索引，锁
     - SQL
       - 语句改写
     - 索引
       - 优化查询
     - 锁
       -
  7. 架构
     - 高可用
       - MHA
     - 高性能
       - 读写分离
       - 分布式
- 优化工具
  - 系统
    - top
    - iostat
  - MySQL
    - show processlist
    - slowlog
    - explain
    - IS, PS, SYS
    - show engine innodb status
    - show status
    - show variables
    - mysqldumpslow
    - pt-toolkits

