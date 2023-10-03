# Compress response

Compress response help reduce the size of response which help the client receive resources in less time, more compression require more server resources but the smaller the size. Gzip come with the Nginx core which we can use to do the job. Gzip have 10 level of compression from 0-9 which the 0 is the original size. The higher the level compression the smaller the size.
The recommended compression level is 3 as the higher compression level, the size is not much different also require more cpu resources.

```bash
http {
    include mime.types;

    gzip on; # Turn on compression.
    gzip_comp_level 3; # Compress level.
    gzip_types text/html text/css text/javascript; # Set compress for html, css, js.

    server {
        listen 80;
        server_name localhost;
        root /var/www/demo;
        try_files $uri $uri/ index.html /404;

        location = /404 {
            return 404 'Sorry, could not found.';
        }
    }
}
```

Request with compression by specified the header.

```bash
curl -I -H "Accept-Encoding: gzip, deflate" http://localhost/index.html
```
