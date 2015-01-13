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


数据库是用来存储和操作数据的对象集合包括表格，数据视图，触发器，存储过程

创建数据库
    CREATE DATABASE [IF NOT EXISTS] database_name;

删除数据库库
    DROP DATABASE [IF EXISTS] database_name;

选择使用数据库
    USE database_name;

数据类型特征：
    1)值所代表的类型
    2)空间消耗，变长或固定长度
    3)数据类型能否被索引
    4)数据类型比较的方式

创建表格
    CREATE TABLE [IF NOT EXISTS] table_name (
        column_name data_type[size] [NOT NULL|NULL] [DEFAULT value]
        [AUTO_INCREMENT] (每张表只能有一个)
        ...

        PRIMARY KEY(col1,col2,...)
    )

    AUTO_INCREMENT必须被索引, 可以是PRIMARY KEY and UNIQUE
            LAST_INSERT_ID()上一个生成的id
        当update更新时,使用更新的自增项
        当delete删除时,相应自增项值将不再使用
        当insert时使用上次自动生成的值加1


主键:
    主键必须唯一
    不能为NULL (mysql默认强制主键为 NOT NULL)
    一张表只能有一列为主键

    为主键列创建索引

    CREATE TABLE users (
       user_id INT AUTO_INCREMENT PRIMARY KEY,
       username VARCHAR(40),
       password VARCHAR(255),
       email VARCHAR(255)
       或者也可以使用  PRIMARY KEY(user_id)
       对于多列主键 PRIMARY KEY(user_id, username)
    )
    
    对与没有主键的表添加主键
    ALTER TABLE table_name ADD PRIMARY KEY(primary_key)

KEY等价于INDEX
UNIQUE索引限制列内不能有重复元素 (可以有多个)
    ALTER TABLE users ADD UNIQUE INDEX emaill_unique(email ASC);

外键
    一个外键可以是一列或者多列
    一张表可以可以有多个外键
    子表中的多个外键可以有多个不同的父表
    one-to-many关系
    子表和父表可以是相同的表

    子表中创建外键
        CONSTRAINT constraint_name
        FOREIGN KEY foreign_key_name(coluns)
        REFERENCES parent_table(columns)
        ON DELETE action(CASCADE, RESTRICT)
        ON UPDATE action

    CREATE TABLE catagories (
        cat_id int not null auto_increment primary key,
        cat_name varchar(255) not null,
        cat_description text
    ) ENGINE=InnoDB
    CREATE TABLE products (
        prd_id int not null auto_increment primary key,
        prd_name varchar(355) not null,
        prd_price decimal,
        cat_id int not null,
        FOREIGN KEY fc_cat(cat_id)
        REFERENCES ccatagories(cat_id)
        ON UPDATE CASCADE
        ON UPDATE RESTRICT
    )

    增加外键
    ALTER  table_name
    ADD CONSTRAINT constraint_name
    REFERENCES parent_table(columns)
    ON DELETE action
    ON UPDATE action


    删除外键
    ALTER TABLE table_name DROP FOREIGN KEY constraint_name

    关闭和开启外键检测
    SET foreign_key_checks = 0
    SET foreign_key_checks = 1
