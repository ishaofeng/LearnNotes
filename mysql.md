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


10.从csv导入数据
    LOAD DATA INFILE 'c:/tmp/discount.csv'
    INTO TABLE discounts
    FIELDS TREMINATED BY ','
    ENCLOSED BY '"'
    LINES TERMINATED by ','
    IGNORE 1 ROWS
    j
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

    CREATE TABLE table1 like table;


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


删除表格
    DROP [TEMPORARY] TABLE [IF EXISTS] table_name [,tablename] ...


查询

SELECT column_1,column_2...
FROM table_1
[INNER | LEFT | RIGHT] JOIN table_2 ON conditions
WHERE conditions
GROUP BY group
HAVING group_conditions
ORDER BY colun_1 [ASC | DESC]
LIMIT offset, row_count

    查询过滤
        1) 条件测试
        2) 使用AND和OR进行条件组合 
        3) 操作符
            BETWEEN 选择一个范围内的值
            LIKE 匹配指定模式
            IN 在值集合中
            IS NULL 检测值是否为空

    查询结果排序
        使用ORDER BY
            对一列或者多列进行排序
            对不同列使用增序或者降序

        SELECT col1, col2...
        FROM tb1
        ORDER BY col1 [ASC|DESC], col2 [ASC|DESC]

        根据表达式计算结果进行排序

        SELECT ordernumber, quantityOrdered * priceEach AS subTotal
        FROM orderdetails
        ORDER BY ordernumber, quantityOrdered * priceEach

        使用自定义顺序
        SELECT orderNumber, status
        FROM orders
        ORDER BY FIELD(status, "In Process", "On Hold"...)

    消除查询结果相同行
        使用DISTINCT操作符消除重复行
        SELECT DISTINCT columns
        FROM table_name
        WHERE where_conditions
        
        当列中有NULL将只保留一个

        SLECT DISTINCT  state
        FROM customers;

        SELECT state
        FROM customers
        GROUP BY state;

        上述两个区别是使用group by的时候对查询结果进行排序

        统计唯一值的个数
        SELECT COUNT(DISTINCT state) 
        FROM customers
        WHERE country = 'USA'
        该查询讲忽略查询结果中的NULL

    使用LIMIT限制查询结果的行数
        SELECT * FROM tb1
        LIMIT offset, count
        
        offset 指定过滤第一行的行号
        count 指定过滤结果的最大行数

    查询数据使用 IN operator
        SELECT column_list
        FROM table_name
        WHERE (expr|column) IN  ("value1", "value2", ...)
        可以在INSERT,UPDATE,DELETE中使用

        如果列表中的值为敞亮,对值进行排序,使用二分查找进行查找

        NOT IN 
        
        对子查询使用 IN
            SELECT  orderNumber,
                    customerNumber,
                    status,
                    shippedDate
            FROM orders
            WHERE orderNumber IN (
                SELECT orderNumber
                FROM orderDetails
                GROUP BY orderNumber
                HAVING SUM(quantityOrdered * priceEach) > 60000
            )

    查询语句使用 BETWEEN  operator
        expr (NOT) BETWEEN begin_expr AND end_expr
        expr必须具有相同的数据类型

        使用cast函数进行显示的类型转换 cast("2014-10-23" as date)

    查询语句使用 LIKE operator 进行匹配
        使用%匹配任意长度的字符串
        使用_匹配单个字符

    MySQL alias
        mysql支持两种alias包括column alias, table alias

        column
            SELECT  CONCAT_WS(',', lastName, firstName) `Full name`
            FROM employees
            ORDER BY `Full name`;

        table
            table_name AS table_alias

    
    Mysql UNION Operator
        将多张表查询的结果合并成为一个结果

        SELECT  column1, column2
        UNION [DISTINCT | ALL]
        SELECT column1, column2
        UNION [DISTINCT | ALL]
        ...

        默认情况下union的结果是去重的, 即使不使用DISTINCT

    Mysql INNER JOIN
        SELECT column_list
        FROM t1
        INNER JOIN t2 ON join_condition1
        INNER JOIN t3 ON join_condition2
        ...
        WHERE where_conditions;
        只包含匹配项

    Mysql LEFT JOIN 
        SELELCT column_list
        FROM t1
        LEFT JOIN t2 ON join_conditions1
        LEFT JOIN t3 ON join_conditions2
        ...
        WHERE where conditions;
        包含匹配项,以及未匹配上的t1该类型的t2,t3字段为NULL

    Mysql Self JOIN
        SELECT  CONCAT(m.lastname, '. ', m.firstname) AS 'Manager',
                CONCAT(e.lastname, '. ', e.firstname) AS 'Direct report'
        FROM employees e
        INNER JOIN employees m ON m.employeeNumber = e.reportsto
        ORDER BY manager


    GROUP BY 
    HAVING 为GROUP指定一个filter condition

    Subquery
        SELECT lastname, firstname
        FROM employees
        WHERE officeCode IN (
            SELELCT officecode
            FROM offices
            WHERE country = 'USA'
        )

    时间戳
        CREATE TABLE ts (
            id INT auto_increment PRIAMRY KEY,
            title VARCHAR(255) NOT NULL,
            create_on TIMESTAMP DEFAULT 0,
            changed_on TIMESTAMP DEFAULT CURRENT_TIMESTAMP
                ON UPDATE CURRENT_TIMESTAMP
        )


随机选择
    SELECT * FROM table ORDER BY RAND() LIMIT N


正则表达式
    SELECT lastname, firstname
    FROM employees
    WHERE lastname REGXP '^(M|B|T)';


Mysql权限管理
    两阶段访问控制
        1) 连接认证  有效的用户名和密码, 发起数据库连接的主机
            必须在数据库中授权
        2) 请求认证 任何一个请求数据库鉴定当前客户端是否由权限执行
           特定的SQL语句


    创建用户
        1) 创建指定主机指定用户
        CREATE USER 'username'@'hostname' IDENTIFIED BY password
        2) 指定用户任意主机
        CREATE USER 'username'@'%' IDENTIFIED BY password
        3) 使用INSERT添加用户
        INSERT INTO user(host, user, password)
        VALUES('localhost', 'dbadmin', PASSWORD("test"))

    更改账户密码
        1) 使用UPDATE
        UPDATE user
        SET password=PASSWORD("pass")
        WHERE user = "mysqltest" AND
              host = "mysqltest.org";
        FLUSH PRIVILEGES;
        2) 使用SET
        SET PASSWORD FOR 'user'@'hostname' = PASSWORD('scret')
        3) 使用grant
        GRANT USAGE ON *.* TO 'user'@'hostname' IDENTIFIED BY 'pass';

    授权
        GRANT priviledges (column_list)
        ON [object_type] privilege_level
        TO accout [IDENTIFIED BY 'password']
        [REQUIRE encryption]
        WITH with_options

        GRANT ALL ON *.* TO 'super'@'localhost' WITH GRANT OPTIONS;
        GRANT ALL classicmodels.* TO 'super2'@'%' WITH GRANT OPTIONS;
        GRANT SELECT, UPDATE, DELETE ON classicmodels.* TO 'rfc'@'%';

    收回授权
        REVOKE privilege_type [(column_list)] [, priv_type[(colun_list)]] ...
        ON [object_type] privileg_level
        FROM user[,user]...
