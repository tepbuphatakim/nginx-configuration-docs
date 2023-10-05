# HTTPS SSL

Secure SSL certificate protocol and setup redirect.

```bash
http {
    include mime.types;

    # Redirect HTTP to HTTPS.
    server {
        listen 80;
        server_name localhost;
        return 301 https://$host$request_uri;
    }

    server {
        listen 443 ssl;
        server_name localhost;
        root /var/www/demo;
        try_files $uri $uri/ /404;

        ssl_certificate /etc/nginx/ssl/self.crt;
        ssl_certificate_key /etc/nginx/ssl/self.key;

        # Use TLS protocol (A newer version of SSL protocol).
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

        # Limit the ciphers to use. To list ciphers (openssl ciphers -V 'HIGH:!aNULL:!MD5').
        ssl_ciphers HIGH:!aNULL:!MD5;
        # Let the client choose their prefer ciphers as modern ciphers already secure.
        ssl_prefer_server_ciphers off;

        # dhparam key size must be the same as SSL certificate key size.
        # Generate dhparam with key size 2048 (openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048).
        ssl_dhparam /etc/nginx/ssl/dhparam.pem;

        # When a browser sees this header from an HTTPS website, it “learns” that this domain must only be accessed using HTTPS (SSL or TLS).
        # It caches this information for the max-age period (typically 31,536,000 seconds, equal to about 1 year).
        # https://www.nginx.com/blog/http-strict-transport-security-hsts-and-nginx/
        add_header Strict-Transport-Security "max-age=31536000" always;

        # SSL cache sessions
        ssl_session_cache shared:SSL:40m;
        ssl_session_timeout 4h;
        ssl_session_tickets on;

        location = /404 {
            return 404 'Sorry, could not found.';
        }
    }
}
```
