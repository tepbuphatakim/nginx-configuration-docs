# Auth basic

Control access using HTTP Basic authentication.
https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/.

Install apache2-utils for generate password.

```bash
apt install apache2-utils
```

Generate password with htpasswd.

```bash
htpasswd -c /etc/nginx/.htpasswd user
```

```bash
http {
    include mime.types;

    server {
        listen 80;
        server_name localhost;
        root /var/www/demo;

        location / {
            auth_basic "Administrator’s Area";
            auth_basic_user_file /etc/nginx/.htpasswd;
            try_files $uri $uri/ index.html /404;
        }
    }
}
```
