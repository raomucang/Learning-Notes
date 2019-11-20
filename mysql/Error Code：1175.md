# Error Code: 1175. You are using safe update

在使用`mysql`执行`update`的时候，如果不是用主键当`where`语句，会报如下错误，使用主键用于`where`语句中正常。
这是因为`MySql`运行在`safe-updates`模式下，该模式会导致非主键条件下无法执行`update`或者`delete`命令，执行命令`SET SQL_SAFE_UPDATES = 0`;修改下数据库模式。
如果想要提高数据库安全等级，可以在恢复回原有的设置，执行命令：`SET SQL_SAFE_UPDATES = 1`;
