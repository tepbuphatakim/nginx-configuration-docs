# Secure

Some steps we can use to enhanced security. Eg, Hide the web server version, update the api repository, etc.

Update and upgrade api repository.

```bash
apt update
apt upgrade
```

Generate password with htpasswd.

```bash
htpasswd -c /etc/nginx/.htpasswd user
```

```bash
http {
    include mime.types;

    # Hide Nginx version.
    server_tokens off;

    server {
        listen 80;
        server_name localhost;
        root /var/www/demo;

        location / {
            try_files $uri $uri/ index.html /404;

            # Prevent iframe embeded from crossorigin.
            add_header X-Frame-Options "SAMEORIGIN";
            # Cross-site scripting protection
            add_header X-XSS-Protection "1; mode=block";
        }
    }
}
```
