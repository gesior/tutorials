
# 7. Linux Configuration

It's worth adjusting a few system configuration options to block basic attacks.

Additionally, you can change network settings to allow the server to handle more connections in case of a DDoS attack.

## 7.1 Configuration Editing

The basic Linux settings configuration is in the file:
```
/etc/sysctl.conf
```

### 7.1.1 Spoof Protection

Find the line with `Spoof protection` and uncomment the configurations below it:
```
#net.ipv4.conf.default.rp_filter=1
#net.ipv4.conf.all.rp_filter=1
```
Replace it with:
```
net.ipv4.conf.default.rp_filter=1
net.ipv4.conf.all.rp_filter=1
```

### 7.1.2 MITM Attacks

Find the line with `MITM attacks` and uncomment the configurations below it:
```
#net.ipv4.conf.all.accept_redirects = 0
#net.ipv6.conf.all.accept_redirects = 0
```
Replace it with:
```
net.ipv4.conf.all.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
```

### 7.1.3 We Are Not a Router

Find the lines with `we are not a router` (there are 2) and uncomment the configurations below them:
```
#net.ipv4.conf.all.send_redirects = 0
```
Replace it with:
```
net.ipv4.conf.all.send_redirects = 0
```
and:
```
#net.ipv4.conf.all.accept_source_route = 0
#net.ipv6.conf.all.accept_source_route = 0
```
Replace it with:
```
net.ipv4.conf.all.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0
```

### 7.1.4 Network Settings

Faster clearing of 'closed connections', reusing the same ports, and 
higher limits for connections/packets waiting to be handled.

Add the following at the end of the file:
```
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.core.netdev_max_backlog = 100000
```

### 7.1.5 Applying Changes

To apply the changes from the `/etc/sysctl.conf` file without restarting Linux, run:
```
sysctl -p
```
