# Cache

Cache is a way to tell a browser how long it should keep the response store in the browser.
Instead of request the resources from the server again the browser will use the resources from cache.
This will help reduce load to the server and improve performance.

```bash
http {
    include mime.types;

    server {
        listen 80;
        server_name localhost;
        root /var/www/demo;

        # Cache any response end with the following extension below for 1 month.
        location ~* \.(css|js|jpg|png)$ {
            access_log off;
            add_header Cache-Control public;
            add_header Pragma public;
            add_header Vary Accept-Encoding;
            expires 1M;
        }
    }
}
```
