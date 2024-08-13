
# 6. nginx Configuration - Website

## 6.1 Default Configuration

By default, nginx loads the website from the `/var/www/html` directory and does NOT support PHP.

The changes we will make:
- load the website from `/home/www`
- add PHP support

## 6.2 Configuration Editing

The nginx website configuration in Ubuntu 20.04 is located in the file:
```
/etc/nginx/sites-enabled/default
```

### 6.2.1 Changing the Directory

Find:
```
root /var/www/html;
```
Replace it with:
```
root /home/www;
```

### 6.2.2 Adding PHP Support

Find:
```
        index index.html index.htm index.nginx-debian.html;
```
Add `index.php`, so it should look like this:
```
        index index.php index.html index.htm index.nginx-debian.html;
```

Find:
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
You need to uncomment the entire block and the lines within it:
```
include snippets/fastcgi-php.conf;
fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
```

So after changes, it should look like this:
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

### 6.2.3 Restarting nginx

To apply the configuration changes, restart nginx:
```
systemctl restart nginx
```
