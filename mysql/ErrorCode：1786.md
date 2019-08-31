# Error Code: 1786 Statement violates GTID consistency: CREATE TABLE ... SELECT.

mysql5.7用命令`create table newtable select * from oldtable` 报错
解决办法：
分两步进行

```mysql
create table newtable like oldtable;
insert into newtable select * from oldtable;
```
