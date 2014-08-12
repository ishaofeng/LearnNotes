#mysql学习
1.备份数据库
 mysqldump -hhostname -uusername -ppassword databasename > backupfile.sql

2.备份数据库为带删除表的格式
  1中加上 --add-drop-table

3.直接将mysql数据库压缩备份
  mysqldump -hhostname -uusername -ppassword databasename | gzip > backupfile.sql.gz

4.备份mysql数据库某些表
  mysqldump -hhostname -uusername -ppassword databasename table1 table2 > backupfile.sql

5.同时备份多个数据库
  mysqldump -hhostname -uusername -ppassword --databases database1 database2 > multibackupfile.sql

6.仅仅备份数据库结构
  mysqldump --no-data --databases database1 database2 database3 > structurebackupfile.sql

7.备份数据库的所有数据
  mysqldump --all-databases > allbackupfile.sql

8.还原Mysql数据库的命令
  mysql -hhostname -uusername -ppassword databse < backupfile.sql

9.还原压缩的mysql数据库
  gunzip < backupfile.sql.gz | mysql -uusername -ppassword database
