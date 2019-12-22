# Mysql 源代码阅读笔记
## 简介
    由于工作中遇到许多sql性能问题，为了突破一些障碍，在业余时间对Mysql一部分源代码进行了学习
    同时为了方便后续回顾，自己也fork了mysql源代码，主要在5.7版本的分支上进行了注释。
## 目录结构
    大概有20多个目前，我们最需要关注的主要有两个目录，sql,storage
    sql里面是除了存储引擎之外mysql的绝大部分逻辑，storage主要是各个存储引擎的功能，比如innodb的页面，锁等机制
## Sql语句执行的主要逻辑流程

```
sequenceDiagram
A->>B: How are you?
B->>A: Great!
```
