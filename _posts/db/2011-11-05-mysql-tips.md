---
layout: post
title: 实用MySQL技巧
---

迁移、导入和导出
-------
- 导出数据库或表
		
		mysqldump -u user -p db_name [table_name] [-w "col_name>100"] > db_name.sql

- 导出特定列数据到csv文件
		
		SELECT col_name FROM table_name INTO OUTFILE 'data.csv'
			FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
  			LINES TERMINATED BY '\n';

- 导入csv文件数据
		
		LOAD DATA INFILE 'data.csv' INTO TABLE table_name
			FIELDS TERMINATED BY ',' ENCLOSED BY '"'
			LINES TERMINATED BY '\r\n'
			IGNORE 1 LINES;

- 导入数据库
		
		mysql -u user -p db_name < db_name.sql

- 重命名数据库

		mysqladmin -u user -p create new_db_name
		mysqldump -u user -p old_db_name | mysql -u user -p new_db_name

