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
        index index.html index.htm index.nginx-debian.html;
```
Dodaj `index.php`, czyli ma wyglądać tak:
```
        index index.php index.html index.htm index.nginx-debian.html;
```

Znajdź:
```
        #location ~ \.php$ {
        #       include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
        #       fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        #}
```
Trzeba odkomentować cały blok i w nim linijki:
```
include snippets/fastcgi-php.conf;
fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
```

Czyli po zmianach ma wyglądać tak:
```
        location ~ \.php$ {
               include snippets/fastcgi-php.conf;
        #
        #       # With php-fpm (or other unix sockets):
               fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        #       # With php-cgi (or other tcp sockets):
        #       fastcgi_pass 127.0.0.1:9000;
        }
```

### 6.2.3 Restart nginx

Aby zmiany w konfiguracji zaczęły działać, należy zrestartować nginx:
```
systemctl restart nginx
```
