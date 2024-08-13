
# 5. PHP Configuration

### 5.1 Default Configuration

The default PHP configuration is roughly OK.

The changes we will make:
- increase the RAM limit for a single request
- increase the file upload size limit

### 5.2 Configuration Editing

The PHP configuration in Ubuntu 20.04 is located in the file:
```
/etc/php/7.4/fpm/php.ini
```

### 5.2.1 Changing the RAM Limit

Find:
```
memory_limit = 128M
```
Replace it with:
```
memory_limit = 512M
```

### 5.2.2 Changing the File Upload Size Limit

Find:
```
post_max_size = 8M
```
Replace it with:
```
post_max_size = 128M
```

Find:
```
upload_max_filesize = 2M
```
Replace it with:
```
upload_max_filesize = 128M
```

### 5.2.3 Restarting PHP

To apply the configuration changes, restart PHP:
```
systemctl restart php7.4-fpm
```
