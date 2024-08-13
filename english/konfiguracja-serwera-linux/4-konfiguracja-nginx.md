# 4. Konfiguracja nginx

### 4.1 Domyślna konfiguracja

Domyślna konfiguracja nginx jest z grubsza OK.

Zmiany, jakie zrobimy:
- dodamy format logowania zapisujący dodatkowe informacje (kraj użytkownika i czas wykonania PHP)
- cachowanie plików logów
- cachowanie otwartych plików
- większy maksymalny rozmiar przesyłanego pliku
- szybsze zrywanie połączeń

Kraj użytkownika będzie logowany tylko po dodaniu CloudFlare.

### 4.2 Edycja konfiguracji

Konfiguracja nginx w Ubuntu 20.04 jest w pliku:
```
/etc/nginx/nginx.conf
```

### 4.2.1 Zmiana ustawień
Znajdź:
```
http {
```
Linijkę niżej dodaj:
```
        log_format cloudflarelog '$remote_addr "$HTTP_CF_IPCOUNTRY" "$http_host" [$time_local] '
        '"$request" $status $body_bytes_sent "$upstream_response_time" "$http_referer" "$http_user_agent"' ;

        open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
        open_file_cache max=200000 inactive=20s;
        open_file_cache_valid 30s;
        open_file_cache_min_uses 2;
        open_file_cache_errors on;

        client_max_body_size 128M;
```

Znajdź:
```
keepalive_timeout 65;
```
Zamień na:
```
keepalive_timeout 5;
```

### 4.2.2 Restart nginx

Aby zmiany w konfiguracji zaczęły działać, należy zrestartować nginx:
```
systemctl restart nginx
```
