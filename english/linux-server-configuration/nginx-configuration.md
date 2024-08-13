
# 4. nginx Configuration

### 4.1 Default Configuration

The default nginx configuration is roughly OK.

The changes we will make:
- add a logging format that records additional information (user's country and PHP execution time)
- log file caching
- open file caching
- increased maximum upload file size
- faster connection termination

The user's country will only be logged after adding CloudFlare.

### 4.2 Configuration Editing

The nginx configuration in Ubuntu 20.04 is located in the file:
```
/etc/nginx/nginx.conf
```

### 4.2.1 Changing Settings

Find:
```
http {
```
Add the following line below it:
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

Find:
```
keepalive_timeout 65;
```
Replace it with:
```
keepalive_timeout 5;
```

### 4.2.2 Restarting nginx

To apply the configuration changes, restart nginx:
```
systemctl restart nginx
```
