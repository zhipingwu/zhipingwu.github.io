# Mysql 源代码阅读笔记

## 简介
    由于工作中遇到许多sql性能问题，为了突破一些障碍，在业余时间对Mysql一部分源代码进行了学习
    同时为了方便后续回顾，自己也fork了mysql源代码，主要在5.7版本的分支上进行了注释。
    
## 目录结构
    大概有20多个目前，我们最需要关注的主要有两个目录，sql,storage
    sql里面是除了存储引擎之外mysql的绝大部分逻辑，storage主要是各个存储引擎的功能，比如innodb的页面，锁等机制
```
BUILD
client
cmake
cmd-line-utils
dbug
Docs
extra
include 头文件：所有后续代码的头文件
libbinlogevents binlog相关的事件：二进制、行、statement
libbinlogstandalone
libmysql
libmysqld
libservices
man 帮助手册
mysql-test
mysys 系统函数的实现：数据类型、缓存、hash、内存、页面、文件、锁等
mysys_ssl
packaging
plugin 插件，密码校验、权限控制等
rapid
regex
scripts
sql sql执行流程相关的所有逻辑
sql-common
storage 存储引擎相关的代码：innodb,myisam等的具体实现
strings
support-files
testclients
unittest 单元测试
vio
win
zlib
CMakeLists.txt
config.h.cmake
configure.cmake
Doxyfile-perfschema
INSTALL
LICENSE
README
VERSION
```
    
## Sql语句执行的主要逻辑流程

```
//代码很长, 入参有个枚举类型 enum_server_command，代表sql语句的类型ddl,dml等
dispatch_command(thd, &com_data, COM_QUERY);

//1491行，解析后的语句会保存在lex里面
mysql_parse(thd, &parser_state);

//执行保存在lex->sql_command里面的sql语句，这个方法写在mysql_parse应该分出来吧？
mysql_execute_command

//执行select操作
res= execute_sqlcom_select(thd, all_tables);

//执行查询：准备、锁表、优化、执行doExec或者解释explain、清理
res= handle_query(thd, lex, result, 0, 0);

//执行prepare
if (select->prepare(thd))

//锁表
lock_tables(thd, lex->query_tables, lex->table_count, 0)

//执行优化器
if (select->optimize(thd))

//执行sql语句
select->join->exec();

//join->exec() 里面会执行如下两个
//join->exec() 1. 查询
Query_result *const query_result= select_lex->query_result();

//join->exec() 2. 发送数据
do_send_rows && query_result->send_data(fields_list)

//清理
res= unit->cleanup(false);

```

## 优化器代码流程
```
//入口
bool SELECT_LEX::optimize(THD *thd)
//首先对join做优化
JOIN::optimize()

//然后循环对每一个unit做优化
if (!unit->is_optimized() && unit->optimize(thd))
//这里会设计到一些代价模型的计算，比如cpu、内存、IO、缓存页、临时表、索引等的代价计算
bool st_select_lex_unit::optimize(THD *thd)

//semi-join MaterializeScan strategy以及其他都会调用这里
//但主要是： bool Optimize_table_order::best_extension_by_limited_search
void Optimize_table_order::best_access_path
```
