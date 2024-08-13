
# 2. Installing nginx, MariaDB, PHP, and Basic Tools

### 2.1 Installing PHP

Install PHP (`php-fpm` for `nginx`) and PHP libraries for Gesior2012:
```
apt install php-fpm php-bcmath php-common php-curl php-json php-gd php-mbstring php-mysql php-pdo php-xml php-zip
```
On systems with PHP versions older than 8.0, you also need to install `php-json`:
```
apt install php-json
```
From version 8.0, `json` is built into PHP and does not need to be installed separately.

### 2.2 Installing nginx

Install nginx:
```
apt install nginx
```

### 2.3 Installing MariaDB

MariaDB is a replacement for MySQL. In many places in Linux, MariaDB configurations are named with `mysql`.

Install MariaDB:
```
apt install mariadb-server mariadb-client
```

### 2.4 Basic Tools

It's worth installing a few basic tools.
They are useful for diagnosing server problems and during attacks.
```
apt install fail2ban iptables-persistent htop nload tcpdump mc zip unzip git screen screenie net-tools moreutils traceroute
```
During the installation, two questions about saving `iptables` may appear. Answer 'No' to both.

What we are installing:
- `fail2ban` - secures SSH, blocks IP after several login attempts with the wrong password
- `iptables-persistent` - allows saving `iptables` rules to a file and loads them after the server restarts
- `htop` - an extended version of `top`
- `nload` - shows the current network usage on the server
- `tcpdump` - captures/displays all incoming/outgoing network packets
- `zip` + `unzip` - programs for packing/unpacking `.zip` files
- `git` - GIT client
- `mc` - graphical file manager operating in the console
- `screen` + `screenie` - applications for managing running `screen`
- `net-tools` - includes `netstat` which allows checking which IP and ports the server is listening on
- `moreutils` - includes `ts`, an application that adds a timestamp in milliseconds to logs (e.g., from OTS console)
- `traceroute` - allows tracing the route of network packets (diagnosing connection problems)
