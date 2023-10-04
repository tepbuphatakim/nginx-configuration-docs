# HTTP2

HTTP2 have varies benefit over HTTP1 including multiplex streamming, binary protocol and compress header.

Nginx configuration details.

```bash
nginx -V
```

List Nginx modules that contain http_v2.

```bash
./configure --help | grep http_v2
```

Configure Nginx with HTTP2 module by specified the module config. HTTP2 required SSL.

```bash
./configure --sbin-path=/usr/sbin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --pid-path=/var/run/nginx.pid --with-http_ssl_module --with-http_v2_module
```

Compile and install.

```bash
make
make install
```

HTTP2 required SSL. Self signed certificate for testing.

```bash
mkdir /etc/nginx/ssl # Create ssl directory in /etc/nginx.
openssl req -x509 -days 90 -nodes -newkey rsa:2048 -keyout /etc/nginx/ssl/self.key -out /etc/nginx/ssl/self.crt # Gen self-signed certificate.
```

Nginx configuration with http2 and self-signed certificate /etc/nginx.conf.

```bash
http {
    include mime.types;

    server {
        listen 443 ssl;
        http2 on;
        server_name localhost;
        root /var/www/demo;
        try_files $uri $uri/ /404;

        ssl_certificate /etc/nginx/ssl/self.crt;
        ssl_certificate_key /etc/nginx/ssl/self.key;

        location = /404 {
            return 404 'Sorry, could not found.';
        }
    }
}
```
