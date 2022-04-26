---
title: Oracle 数据库日常维护
date: 2018-11-21 14:03:16
tags: [Database,DevOps]
categories: 工程技术
---
## 摘要
- 【编辑中】
- init params
- files tree

<!--more-->

## INSTALL

#### Security

```SQL
sql>alter user USER_NAME identified by USER_PASSWD;

--查看用户的proifle，默认：default
sql>SELECT username,PROFILE FROM dba_users where username='admin' ;

sql>SELECT * FROM dba_profiles s WHERE s.profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME';

sql>ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
sql>ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME 90;
```

#### Init Params

- 初始化参数文件（Initialization Parameters Files,pfile）
pfile 默认为“init+例程名.ora”,文本文件，可以手工编辑。

- spfile:服务器参数文件（Server Parameter Files）
spfile 默认为“spfile+例程名.ora”，**不能用vi编辑器对其中参数进行修改，只能通过命令在线修改。**

```sql
-- $ORACLE_HOME/dbs/spfiledbnms.ora
--pfile格式转换：将二进制转换为文本格式
SQL> CREATE Spfile FROM pfile;
这种方法，然后再用vi编辑器对其中的参数进行直观修改，以达到方便的目的。

--pfile格式转换：将文本转为二进制格式
SQL> CREATE pfile FROM Spfile;

# df -k /dev/shm
Filesystem     1K-blocks  Used Available Use% Mounted on
tmpfs            4194304   288   4194016   1% /dev/shm

```

- [SGA](https://docs.oracle.com/cd/B19306_01/server.102/b14237/initparams193.htm#REFRN10256)
- [pga_aggregate_target](https://docs.oracle.com/cd/B19306_01/server.102/b14237/initparams157.htm#REFRN10165)

SGA_TARGET specifies the total size of all SGA components.
If SGA_TARGET is specified, then the following memory pools are automatically sized:

- Buffer cache (DB_CACHE_SIZE)
- Shared pool (SHARED_POOL_SIZE)
- Large pool (LARGE_POOL_SIZE)
- Java pool (JAVA_POOL_SIZE)
- Streams pool (STREAMS_POOL_SIZE)

```
pga_aggregate_target = total memory - SGA

```

When setting this parameter, you should examine the total memory on your system that is available to the Oracle instance and subtract the SGA. You can assign the remaining memory to PGA_AGGREGATE_TARGET.

```perl
# example:vi $ORACLE_HOME/dbs/spfiledbnms.ora
# dbnms.__java_pool_size=939524096
# dbnms.__large_pool_size=805306368
dbnms.__java_pool_size=4294967296
dbnms.__large_pool_size=4294967296
dbnms.__oracle_base='/oracle/product'#ORACLE_BASE set from environment
#dbnms.__pga_aggregate_target=16106127360
dbnms.__pga_aggregate_target=0
#dbnms.__sga_target=64424509440
dbnms.sga_target=4294967296
dbnms.__shared_io_pool_size=0
dbnms.__shared_pool_size=4697620480
dbnms.__streams_pool_size=0
*.audit_file_dest='/oracle/product/admin/dbnms/adump'
*.audit_trail='db'
*.compatible='11.2.0.4.0'
*.control_files='/oradata1/dbnms/control01.ctl','/oradata2/dbnms/control02.ctl'
*.db_block_size=8192
*.db_domain=''
*.db_name='dbnms'
*.diagnostic_dest='/oracle/product'
*.dispatchers='(PROTOCOL=TCP) (SERVICE=dbnms)'
*.open_cursors=300
#*.pga_aggregate_target=16106127360
*.pga_aggregate_target=0
*.processes=1500
*.remote_login_passwordfile='EXCLUSIVE'
*.sessions=1655
# *.sga_target=64424509440
*.sga_target=4294967296
*.undo_tablespace='UNDOTBS1'
```

## Optimizing

#### Metric

```sql
select 'vsession:'||count(*)||'' as moni from v$session
union
select 'maxsessions:'||value||'' as sessions from v$parameter where name = 'sessions'
union
select 'vprocess:'||count(*)||'' from v$process
union
select 'maxprocess:'||value||'' from v$parameter where name = 'processes'
```

#### Events

```SQL
select * from v$event_name;
select * from v$sql;


select p.spid, s.osuser, s.program ,p.* from v$session s,v$process p where s.paddr=p.addr   and s.SID='8463';


---eg:Buffer Busy Waits
select   sql_text   
from   v$sql t1, v$session t2, v$session_wait   t3  
where   1=1
and t1.address=t2.sql_address   
and t1.hash_value=t2.sql_hash_value
and t2.sid=t3.sid   
and t3.event='buffer busy waits';

```

#### SQL

- TOP IO SQL

```SQL
-- - TOP IO SQL
select p.spid,s.sid,s.machine,s.program,q.disk_reads,q.sql_text
from v$process p,v$session s,v$sql q
where p.addr=s.paddr and s.sql_id=q.sql_id
order by 5;
```

#### INDEX

```SQL

analyze table TABLENAME compute statistics;  

select sid,opname,target,sofar,totalwork,trunc(sofar/totalwork*100,2)||'%' as perwork
from v$session_longops where sofar!=totalwork and sid='1479';

-- 21:16 1	1479	Table Scan	SLVIEW.FLUX	6457	128417	5.02%
-- 1	1479	Table Scan	TEST.TABLE_A	14813	128417	11.53%
```

#### Lock

```sql

select 'alter system kill session '''||''||s.sid||','||s.serial#||''';'
from v$locked_object lo,dba_objects ao,v$session s
where ao.object_id = lo.object_id and lo.session_id = s.sid
and object_name = 'TABLENAME';

select /*+ rule */ s.username,decode(l.type,'TM','TABLE LOCK','TX','ROW LOCK',NULL) LOCK_LEVEL,
o.owner,o.object_name,o.object_type,s.sid,s.serial#,s.terminal,s.machine,s.program,s.osuser,l.CTIME
FROM v$session s,v$lock l,dba_objects o WHERE l.sid = s.sid AND l.id1 = o.object_id(+)
and machine ='HOST_NAME'
and owner='USER_NAME'  
and object_name='TABLE_NAME'


select t2.username,  
       t2.sid,  
       t2.serial#,  
       t3.object_name,  
       t2.OSUSER,  
       t2.MACHINE,  
       t2.PROGRAM,  
       t2.LOGON_TIME,  
       t2.COMMAND,  
       t2.LOCKWAIT,  
       t2.SADDR,  
       t2.PADDR,  
       t2.TADDR,  
       t2.SQL_ADDRESS,  
       t1.LOCKED_MODE  
  from v$locked_object t1, v$session t2, dba_objects t3  
where t1.session_id = t2.sid  
   and t1.object_id = t3.object_id  
order by t2.logon_time;
```

#### Constraint

```SQL
select a.constraint_name, a.table_name, b.constraint_name
from user_constraints a, user_constraints b
where
a.constraint_type = 'R' --外键
--and b.constraint_type = 'P' --主键
and a.r_constraint_name = b.constraint_name
and a.constraint_name like '%KEY_NAME%';

-- 启用外键约束
alter table table_name enable constraint constraint_name
-- 禁用外键约束
alter table table_name disable constraint constraint_name

select 'alter table '||table_name||' enable constraint '||constraint_name||';' from user_constraints where constraint_type='R';

select 'alter table '||table_name||' disable constraint '||constraint_name||';' from user_constraints where constraint_type='R' ;

```

## 数据迁移

#### 数据泵

```bash
## 第一步：迁移准备，默认参数
select * from dba_directories;

create directory dir_dp  as '/oradata1/backdp'

## 第二步:备份数据
expdp 'dbusername/******#@dbnms' directory=DIR_DP dumpfile=LOG_P_20200930.dmp tables=USERLOGINFO:P_20200930 logfile=LOG_EXP_20200930.log &

## 第三步：Rename历史表
rename USERLOGINFO to USERLOGINFO_20200910;



## 第四步：重建表数据
impdp 'dbusername/******#@dbnms' directory=DIR_DP dumpfile=LOG_P_20200930.dmp remap_tablespace=DATALIST:DATALOG logfile=LOG_IMP_20200930.log &

impdp 'dbusername/******@@dbnms' directory=DIR_DP dumpfile=LOG_P_20200831.dmp remap_tablespace=DATALIST:DATALOG logfile=LOG_IMP_20200831.log table_exists_action=append &
```

#### 注意事项

- **REMAP_TABLE feature was only introduced in 11G**

### Expdp

```bash
# more LOG_EXP_20200930.log
;;;
Export: Release 10.2.0.4.0 - 64bit Production on Thursday, 10 September, 2020 23:25:16

Copyright (c) 2003, 2007, Oracle.  All rights reserved.
;;;
Connected to: Oracle Database 10g Enterprise Edition Release 10.2.0.4.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
Starting "DBUSER"."SYS_EXPORT_TABLE_04":  dbusername/********@dbnms directory=DIR_DP dumpfile=LOG_P_20200930.dmp tables=USERLOGINFO:P_20200930 logfile=LOG_EXP_20200930.log
Estimate in progress using BLOCKS method...
Processing object type TABLE_EXPORT/TABLE/TABLE_DATA
Total estimation using BLOCKS method: 4.994 GB
Processing object type TABLE_EXPORT/TABLE/TABLE
Processing object type TABLE_EXPORT/TABLE/INDEX/INDEX
Processing object type TABLE_EXPORT/TABLE/INDEX/STATISTICS/INDEX_STATISTICS
Processing object type TABLE_EXPORT/TABLE/STATISTICS/TABLE_STATISTICS
. . exported "DBUSER"."USERLOGINFO":"P_20200930"         4.335 GB 46060706 rows
Master table "DBUSER"."SYS_EXPORT_TABLE_04" successfully loaded/unloaded
******************************************************************************
Dump file set for SLVIEW.SYS_EXPORT_TABLE_04 is:
  /oradata1/backdp/LOG_P_20200930.dmp
Job "DBUSER"."SYS_EXPORT_TABLE_04" successfully completed at 23:47:46
```

### Impdp

```bash
$ more LOG_IMP_20200930.log
;;;
Import: Release 10.2.0.4.0 - 64bit Production on Friday, 11 September, 2020 0:17:07

Copyright (c) 2003, 2007, Oracle.  All rights reserved.
;;;
Connected to: Oracle Database 10g Enterprise Edition Release 10.2.0.4.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
Master table "DBUSER"."SYS_IMPORT_FULL_01" successfully loaded/unloaded
Starting "DBUSER"."SYS_IMPORT_FULL_01":  dbusername/********@dbnms directory=DIR_DP dumpfile=LOG_P_20200930.dmp remap_tablespace=DATALIST:DATALOG logfile=LOG_IMP_20200930.log
Processing object type TABLE_EXPORT/TABLE/TABLE
Processing object type TABLE_EXPORT/TABLE/TABLE_DATA
. . imported "DBUSER"."USERLOGINFO":"P_20200930"         4.335 GB 46060706 rows
Processing object type TABLE_EXPORT/TABLE/INDEX/INDEX
ORA-31684: Object type INDEX:"SLVIEW"."IND_USERLOGINFO_LOGTIME" already exists
Processing object type TABLE_EXPORT/TABLE/INDEX/STATISTICS/INDEX_STATISTICS
ORA-39111: Dependent object type INDEX_STATISTICS skipped, base object type INDEX:"DBUSER"."IND_USERLOGINFO_LOGTIME" already exists
Processing object type TABLE_EXPORT/TABLE/STATISTICS/TABLE_STATISTICS
```

#### Impdp参数
```
REMAP_SCHEMA=scott:system   更改owner
TABLESPACES=example   导入表空间
TABLE_EXISTS_ACTION   追加数据
table_exists_action   
{
      skip 如果已存在表，则跳过并处理下一个对象；
      append 为表增加数据；
      truncate 截断表，然后为其增加新数据；
      replace 删除已存在表，重新建表并追加数据
}
```

### 迁移过程

#### 监控利用率

```bash

SELECT a.tablespace_name "表空间名",
round(total) "表空间大小",
free "表空间剩余大小",
round((total - free)) "表空间使用大小",
round(total / (1024 * 1024 * 1024)) "表空间大小(G)",
round(free / (1024 * 1024 * 1024)) "表空间剩余大小(G)",
round((total - free) / (1024 * 1024 * 1024)) "表空间使用大小(G)",
round((total - free) / total, 4) * 100 "使用率 %"
FROM (SELECT tablespace_name, SUM(bytes) free
FROM dba_free_space
GROUP BY tablespace_name) a,
(SELECT tablespace_name, SUM(bytes) total
FROM dba_data_files
GROUP BY tablespace_name) b
WHERE a.tablespace_name = b.tablespace_name
--and round((total - free) / total, 4) * 100 > 80
order by round((total - free) / total, 4) * 100 desc
```

#### 调整表空间

```bash

ALTER TABLESPACE DATAFLUX ADD DATAFILE '/oradata6/dbnms/dataflux601.dbf' SIZE  4096M;

--1、查看表空间的名称及大小
SELECT t.tablespace_name, round(SUM(bytes / (1024 * 1024)), 0) ts_size
FROM dba_tablespaces t, dba_data_files d
WHERE t.tablespace_name = d.tablespace_name
GROUP BY t.tablespace_name;
--2、查看表空间物理文件的名称及大小
SELECT tablespace_name,
file_id,
file_name,
round(bytes / (1024 * 1024), 0) total_space
FROM dba_data_files
where  tablespace_name='DATACFG'
ORDER BY tablespace_name;

select * from dba_data_files;

--3、查看回滚段名称及大小
SELECT segment_name,
tablespace_name,
r.status,
(initial_extent / 1024) initialextent,
(next_extent / 1024) nextextent,
max_extents,
v.curext curextent
FROM dba_rollback_segs r, v$rollstat v
WHERE r.segment_id = v.usn(+)
ORDER BY segment_name;
--4、查看控制文件
SELECT NAME FROM v$controlfile;
--5、查看日志文件
SELECT MEMBER FROM v$logfile;
--6、查看表空间的使用情况
SELECT SUM(bytes) / (1024 * 1024) AS free_space, tablespace_name
FROM dba_free_space
GROUP BY tablespace_name;
SELECT a.tablespace_name,
a.bytes total,
b.bytes used,
c.bytes free,
(b.bytes * 100) / a.bytes "% USED ",
(c.bytes * 100) / a.bytes "% FREE "
FROM sys.sm$ts_avail a, sys.sm$ts_used b, sys.sm$ts_free c
WHERE a.tablespace_name = b.tablespace_name
AND a.tablespace_name = c.tablespace_name;
--7、查看数据库库对象
SELECT owner, object_type, status, COUNT(*) count#
FROM all_objects
GROUP BY owner, object_type, status;
--8、查看数据库的版本　
SELECT version
FROM product_component_version
WHERE substr(product, 1, 6) = 'Oracle';
--9、查看数据库的创建日期和归档方式
SELECT created, log_mode, log_mode FROM v$database;
```



## 参考文献
- [Oracle initparams](https://docs.oracle.com/cd/B19306_01/server.102/b14237/initparams.htm#REFRN001)
- [Oracle GoldenGate Doc](https://docs.oracle.com/en/middleware/goldengate/index.html)
- [Oracle GoldenGate 产品介绍](https://www.oracle.com/technetwork/cn/community/developer-day/3-goldengate-289794-zhs.pdf)
- [Using Oracle GoldenGate Microservices Architecture](https://docs.oracle.com/goldengate/c1230/gg-winux/GGSAU/GGSAU.pdf)
