# 6. Konfiguracja nginx - strona www

## 6.1 Domyślna konfiguracja

Domyślnie nginx ładuje stronę z katalogu `/var/www/html` i NIE obsługuje PHP.

Zmiany, jakie zrobimy:
- ładowanie strony z `/home/www`
- dodanie obsługi PHP

## 6.2 Edycja konfiguracji

Konfiguracja strony www nginx w Ubuntu 20.04 jest w pliku:
```
/etc/nginx/sites-enabled/default
```

### 6.2.1 Zmiana katalogu
Znajdź:
```
root /var/www/html;
```
Zamień na:
```
root /home/www;
```

### 6.2.2 Dodanie obsługi PHP
Znajdź:
```
#       include snippets/fastcgi-php.conf;
```
Odkomentuj, czyli ma wyglądać tak:
```
       include snippets/fastcgi-php.conf;
```

Znajdź:
```
#       fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
```
Odkomentuj, czyli ma wyglądać tak:
```
       fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
```

### 6.2.3 Restart nginx

Aby zmiany w konfiguracji zaczęły działać, należy zrestartować nginx:
```
systemctl restart nginx
```
