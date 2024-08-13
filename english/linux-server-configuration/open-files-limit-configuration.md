
# 8. Configuring the Open Files Limit

In Linux, open connections (between the player and the server) are counted as open files.
The default limit for open files is 1024.
If someone opens 1024 connections to the game server/web server, no one else will be able to join or open the page.

Alternatively, your server might become super popular, and suddenly, with 1000 online users, you'll start getting 'server offline' errors when someone tries to log in.

The number of open files for a given application is limited by three configurations:
- `/etc/security/limits.conf` - limits the number of open files per Linux user
- `/etc/systemd` - limits the number of open files per application
- there is also a general system-wide open files limit, but on newer Linux systems, it is set quite high by default

I increase the limit to __300,000__.
The RAM will run out, or the CPU will be 100% loaded long before any application reaches that number of connections.

## 8.1 Editing the User Open Files Limit

We will edit the open files limit for three users:
- `root` - used to run OTS
- `mysql` - the user under which MariaDB operates
- `www-data` - the user under which `nginx` and `php` operate

If you run OTS from a user other than `root`, set high limits for that user as well.

The limits are set in the file:
```
/etc/security/limits.conf
```

At the end of the file, add:
```
mysql           hard    nofile          300000
mysql           soft    nofile          150000

www-data        hard    nofile          300000
www-data        soft    nofile          150000

root            hard    nofile          300000
root            soft    nofile          150000
```
(`nofile` is short for `number of files`)

## 8.2 Editing the Open Files Limit per Application

Each application has its configuration somewhere in `/etc/systemd`.
You need to find the configuration files for the given application and edit them.

### 8.2.1 Edit nginx Application Limits

Run the following to find the `nginx` configuration files:
```
find /etc/systemd -name 'nginx*'
```
Open each found file.

Look for the line that starts with:
```
LimitNOFILE=
```
If it exists, replace it with:
```
LimitNOFILE=300000
```
If it doesn't exist, find:
```
[Service]
```
and add the following line below it:
```
LimitNOFILE=300000
```

### 8.2.2 Do the Same for PHP and MariaDB

You can find the PHP configuration by running:
```
find /etc/systemd -name 'php*'
```
You can find the MariaDB configuration by running:
```
find /etc/systemd -name 'mariadb*'
```
### 8.2.3 Restart the Server

You can try to reload all the changed permissions using commands,
but it's much easier to do it by restarting Linux:
```
reboot
```
