
# 3. MariaDB Configuration

### 3.1 Default Configuration

The default MariaDB configuration typically uses around 400 MB of RAM.

There are many limits set that prevent calculations from being performed in RAM, and
many database queries end up creating a 'temporary file on disk' - 
which significantly slows down MariaDB.
Additionally, with the default configuration, data is usually loaded from disk.

If you have some free RAM on the server, it's worth changing this.

### 3.2 Optimization - Editing the Configuration

Optimizing MariaDB configuration is very complex.
Below is an example configuration designed for large OTSs.

__WARNING!__
If you want to optimize the database of an already running OTS,
make sure to back up first!

The MariaDB configuration in Ubuntu 20.04 is located in the file:
```
/etc/mysql/mariadb.conf.d/50-server.cnf
```
Example changes in the configuration after which MariaDB should use 4-8 GB of RAM.
Recommended for any server with 16 GB RAM or more.

#### 3.2.1 Changing Basic Limits

Find:
```
#key_buffer_size        = 16M
#max_allowed_packet     = 16M
#thread_stack           = 192K
#thread_cache_size      = 8
```
Uncomment and change the values to:
```
key_buffer_size        = 512M
max_allowed_packet     = 512M
thread_stack           = 1M
thread_cache_size      = 128
```

#### 3.2.2 Changing Connection and Table Limits

Set high values that the server should never reach.

Find:
```
#max_connections        = 100
#table_cache            = 64
```
Uncomment and change the values to:
```
max_connections        = 50000
table_cache            = 50000
```

#### 3.2.3 Changing Query Limits Processed in RAM

Add:
```
max_heap_table_size     = 1G
tmp_table_size          = 1G
bulk_insert_buffer_size = 1G
```

#### 3.2.4 Changing Limits for InnoDB

InnoDB is the MariaDB engine used by the OTS database.

These changes have the greatest impact on the OTS save time.

Add:
```
innodb_file_per_table = 1
innodb_buffer_pool_size = 4G
innodb_buffer_pool_instances = 8
innodb_log_file_size = 512M
innodb_log_buffer_size = 256M
innodb_flush_log_at_trx_commit = 2
```

#### 3.2.5 SQL Mode - Issues with Acc. Makers

Acc. makers, e.g., Gesior2012, do not adhere to SQL standards.
For many years this was not a problem,
because the default settings allowed breaking some SQL rules.
This has changed in the new MySQL and MariaDB.

Fortunately, you can switch the mode to work as it did before,
by adding the following line to the configuration:
```
sql_mode=''
```

#### 3.2.6 Restarting MariaDB

To apply the configuration changes, restart MariaDB:
```
systemctl restart mysql
```
(yes, you write `mysql`, but it restarts MariaDB)

### 3.3 Database Access

By default, the MariaDB database is accessible from the console by the `root` user without a password.
We need to change the user access so that you can log in with a password.

In the example, we will set the `root` user password to `secretpass`.

#### 3.3.1 Logging in with a Password

Run:
```
mysql
```
This will start the MariaDB server console.

Run the following commands in sequence:
1. Select the database containing the users:
```
use mysql;
```
2. Change the login type from `unix_socket`
(logging in based on the console user)
to password login:
```
UPDATE `user` SET `plugin` = 'mysql_native_password' WHERE `user` = 'root' AND `host` = 'localhost';
```
3. Set the `root` user password to `secretpass`:
```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('secretpass');
```
4. Reload MariaDB user settings:
```
flush privileges;
```
5. Exit the MariaDB console (you can also press CTRL+D)
```
exit
```

#### 3.3.2 Automatic Console Login

After setting the password, to log in to MariaDB from the console, you need to enter:
```
mysql -p
```
and then enter the password.

You can enter the password in the MariaDB configuration.
After this change, the `mysql` and `mysqldump` commands will no longer require entering a password.

Edit the file:
```
/etc/mysql/mariadb.conf.d/50-client.cnf
```
Under:
```
[client]
```
Add the line:
```
password=secretpass
```
After this change, you can again use `mysql` (without `-p`) without entering a password.
