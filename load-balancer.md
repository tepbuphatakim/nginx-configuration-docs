# Load balancer

```bash
http {
    upstream servers {
        server localhost:81;
        server localhost:82;
        server localhost:83;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://servers;
        }
    }
}
```
