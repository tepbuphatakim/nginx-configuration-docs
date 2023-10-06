# Rate limiting

Rate limiting allow us to control and limit the incoming traffic also add extra layers for security.

Install siege for simulate load testing to http.

```bash
apt install siege
```

Making 5 requests with 5 concurrent test.

```bash
siege -v -r 5 -c 5 https://localhost
```

```bash
http {
    include mime.types;

    limit_req_zone $binary_remote_addr zone=RATELIMIT:10m rate=60r/s;

    server {
        listen 80;
        server_name localhost;
        root /var/www/demo;

        location / {
            limit_req zone=RATELIMIT burst=60 nodelay;
            try_files $uri $uri/ index.html /404;
        }

        location = /404 {
            return 404 'Sorry, could not found.';
        }
    }
}
```
