> MooC：
>
> [数据库系统概论 西安电子科技大学](https://www.icourse163.org/course/XDU-1002199005)
>
> 教科书：
>
> Abraham Silberschatz, Henry F. Korth, S.Sudarshan, Database System Concepts (6th ed.), The McGraw-Hill Companies, 2011
>
> 参考教材：
>
> 1. Thomas M. Connolly, et al. Database Systems: A practical approach to Design, Implementation, and Management. 6th edition, Addison-Wesley, 2014 
>
> 2. 王珊，萨师煊《数据库系统概论》 高等教育出版社(第5版),2014

## 一、绪论

## 1. 数据库简介

### 数据(Data)

### 数据库(Database)

- 相互有关系的数据的集合

### 数据库管理系统(DBMS)

- 
- 满足
  - 数据可以共享
  - 避免数据冗余
  - 避免数据不一致性
  - 提供事务支持
  - 保证完整性
- 功能
  - Data defination(Data Definition Language——DDL)
    - s
  - Data manipulation(Data Manipulation Language DML)
    - 用户利用DML实现对数据库数据的基本操作，如查询、插入、删除和修改等
  - Database running mangement
    - 在数据库的建立、运行和维护时，由DBMS进行统一管理、统一控制，以确保数据的安全性、完整性、多用户的并发操作，以及发生故障后的系统恢复

### 模式，外模式，内模式(Schema, External Schema, Physcial Schema)

- 模式
  - 数据库中全体数据的逻辑结构和特征的描述
- 外模式
  - 数据库用户使用的局部数据的逻辑结构与特征的描述
- 内模式
  - 数据物理结构和存储方式的描述

### 数据库系统(Database System)

- 数据(Data)
  - 特性
    - 共享的
      - 不同的用户都可以访问
- 硬件(Hardware)
  - Processor and Main memory
  - Secondary storage volume
    - I/O device...
- 软件(Software)
  - OS
  - Database Management System
  - Other Software
- 人(Peope)
  - End Users
    - Casual users
    - Naive users
  - Application programmers
  - DBA(Database administrator)

## 2 数据库发展历史

文件系统(File System)$\to$数据库管理系统

### 文件系统(File System)

1960s之前，数据库应用建立在文件系统之上

### 文件系统的缺点

- 数据的冗余与不一致性(redundancy and inconsistency)
- 访问数据的困难(diffcultiy of accessing data)
- 数据的隔离(isolation)
- 完整性(integrity)
- 更新的原子性(atomicity)
- 多个用户访问同一文件
- 安全问题(security)

### 数据库管理系统(DBMS)

数据库管理系统解决了以上文件系统无法解决的所有问题

#### 发展阶段

- 1960s
  - hierarchical data model

- 1960s
  - network data model
- 1970s 
  - Relational system
- 1980s 
  - Object-Relation system
- 1990s 
- 2000s
  - XML and XQuery standards
  - Automated database administration
- 2010s
  - Bigdata and NoSQL

### 数据库领域的四位图灵奖得主

## 二、关系代数

