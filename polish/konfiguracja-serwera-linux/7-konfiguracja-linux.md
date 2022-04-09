# 7. Konfiguracja Linux

Warto przestawić kilka opcji w konfiguracji systemu, żeby zablokować podstawowe ataki.

Dodatkowo możesz zmienić ustawienia sieci,
żeby serwer był w stanie obsługiwać więcej połączeń w razie ataku DDoS.

## 7.1 Edycja konfiguracji

Konfiguracja podstawowych ustawień Linux jest w pliku:
```
/etc/sysctl.conf
```

### 7.1.1 Spoof protection

Znajdź linijkę z `Spoof protection` i odkomentuj konfiguracje pod nią, czyli:
```
#net.ipv4.conf.default.rp_filter=1
#net.ipv4.conf.all.rp_filter=1
```
Zamień na:
```
net.ipv4.conf.default.rp_filter=1
net.ipv4.conf.all.rp_filter=1
```

### 7.1.2 MITM attacks

Znajdź linijkę z `MITM attacks` i odkomentuj konfiguracje pod nią, czyli:
```
#net.ipv4.conf.all.accept_redirects = 0
#net.ipv6.conf.all.accept_redirects = 0
```
Zamień na:
```
net.ipv4.conf.all.accept_redirects = 0
net.ipv6.conf.all.accept_redirects = 0
```

### 7.1.3 we are not a router

Znajdź linijki z `we are not a router` (są 2) i odkomentuj konfiguracje pod nimi, czyli:
```
#net.ipv4.conf.all.send_redirects = 0
```
Zamień na:
```
net.ipv4.conf.all.send_redirects = 0
```
i:
```
#net.ipv4.conf.all.accept_source_route = 0
#net.ipv6.conf.all.accept_source_route = 0
```
Zamień na:
```
net.ipv4.conf.all.accept_source_route = 0
net.ipv6.conf.all.accept_source_route = 0
```

### 7.1.4 Ustawienia sieci

Szybsze kasowanie 'zamkniętych połączeń', ponowne używanie tych samych portów i 
większe limity połączeń/pakietów czekających na obsłużenie.

Na końcu pliku dodaj:
```
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_tw_reuse = 1
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.core.netdev_max_backlog = 100000
```

### 7.1.5 Załadowanie zmian

Aby załadować zmiany z pliku `/etc/sysctl.conf` bez restartu Linuxa odpal:
```
sysctl -p
```
