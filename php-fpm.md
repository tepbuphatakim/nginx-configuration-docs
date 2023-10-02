# PHP Configuration

Check systemd service and search only result contain php.

```bash
systemctl list-units | grep php
```

Check service status.

```bash
systemctl status php8.1-fpm
```

Find directory of PHP fpm sock file.

```bash
find / -name *fpm.sock
```

Check running process details.

```bash
ps aux | grep nginx
```

Nginx configuration /etc/nginx/nginx.conf.

```bash
user www-data; # Run Nginx as www-data user (To avoid permission problems).

http {
    include mime.types;

    server {
        listen 80;
        server_name localhost;
        root /var/www/demo;
        # Priority index page from left to right.
        index index.php index.html;

        location / {
            try_files $uri $uri/ =404;
        }

        # Forward URI request end with php for PHP fpm to process.
        location ~\.php$ {
            include fastcgi.conf;
            fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        }
    }
}
```
