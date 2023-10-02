# Worker processes

Configure Nginx to run processes according to a given configuration.
By changing this configuration you can set the Nginx to run at maximum hardware power.

```bash
pid /var/run/custom.pid; # Reconfigure pid directory.
worker_processes auto; # Set Nginx worker instance according to CPU core.

events {
    worker_connections 1024; # Set maximum connections at a times.
}

http {}
```
