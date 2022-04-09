# 5. Konfiguracja PHP

### 5.1 Domyślna konfiguracja

Domyślna konfiguracja PHP jest z grubsza OK.

Zmiany, jakie zrobimy:
- zwiększymy limit RAM dla pojedynczego zapytania
- zwiększymy limit rozmiaru przesyłanych plików

### 5.2 Edycja konfiguracji

Konfiguracja PHP w Ubuntu 20.04 jest w pliku:
```
/etc/php/7.4/fpm/php.ini
```

### 5.2.1 Zmiana limitu RAM
Znajdź:
```
memory_limit = 128M
```
Zamień na:
```
memory_limit = 512M
```

### 5.2.2 Zmiana limitu przesyłanych plików
Znajdź:
```
post_max_size = 8M
```
Zamień na:
```
post_max_size = 128M
```

Znajdź:
```
upload_max_filesize = 2M
```
Zamień na:
```
upload_max_filesize = 128M
```

### 5.2.3 Restart PHP

Aby zmiany w konfiguracji zaczęły działać, należy zrestartować PHP:
```
systemctl restart php7.4-fpm
```
