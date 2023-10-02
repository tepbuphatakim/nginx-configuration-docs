# Nginx Ubuntu Guide

## Install from sources

From http://nginx.org/en/docs/install.html.
If some special functionality is required, not available with packages and ports, nginx can also be compiled from source files.
While more flexible, this approach may be complex for a beginner.
For more information, see [Building nginx from Sources](http://nginx.org/en/docs/configure.html).

The command to download Nginx source code. Link can be found in nginx.org download tab.

```bash
wget http://nginx.org/download/nginx-1.25.2.tar.gz
```

Extract source.

```bash
tar -zxvf nginx-1.25.2.tar.gz
```

Install source.

In the extract source. Check compiler requirement.

```bash
./configure
```

Install compiler and relate modules.

```bash
apt install build-essential
apt install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
```

Configure source.

```bash
./configure --sbin-path=/usr/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --with-pcre --pid-path=/var/run/nginx.pid --with-http_ssl_module
```

Compile running source and install.

```bash
make
make install
```

## Setup systemd for manage Nginx

Reference https://www.nginx.com/resources/wiki/start/topics/examples/systemd/.
In /lib/systemd/system/nginx.service config systemd acording to your system path.

```bash
[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

## Configuration

In the /etc/nginx/nginx.conf. Simple configuration for vitual host.
The include mine.types is to map the Content-Type response header according to file extension.

```bash
http {
    include mime.types;

    server {
        listen       80;
        server_name  localhost;
        root /var/www/demo;
    }
}
```

Nginx URI mapping with location block.

```bash
http {
    include mime.types;

    server {
        listen       80;
        server_name  localhost;
        root /var/www/demo;

        location /prefix {
            return 200 'Nginx prefix match.';
        }

        location = /exact {
            return 200 'Nginx exact match.';
        }

        location ~ /regex[0-9] {
            return 200 'Nginx regex match 0 - 9';
        }
    }
}
```

Nginx build-in variables.

```bash
http {
    include mime.types;

    server {
        listen       80;
        server_name  localhost;
        root /var/www/demo;

        location = /var {
            return 200 'Host: $host\n URI: $uri\n Query: $args';
        }
    }
}
```

Nginx redirect, redirect to new url.

```bash
http {
    include mime.types;

    server {
        listen       80;
        server_name  localhost;
        root /var/www/demo;

        location = /redirect {
            return 307 /uri;
        }
    }
}
```

Nginx try files. Try files will looking from the argument from left to right relative with the root until the last argument.
In this case rewrite to 404 location.

```bash
http {
    include mime.types;

    server {
        listen       80;
        server_name  localhost;
        root /var/www/demo;

        try_files $uri /404;

        location = /404 {
            return 404 'Sorry, could not found.';
        }
    }
}
```

Logging management.

```bash
http {
    include mime.types;

    server {
        listen       80;
        server_name  localhost;
        root /var/www/demo;

        location = /log {
            access_log /var/log/nginx/custom.access.log;
            # access_log off; Too disabled log
            return 200 'Log.';
        }
    }
}
```
