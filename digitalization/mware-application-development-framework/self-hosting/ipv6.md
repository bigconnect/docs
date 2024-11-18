# IPv6

### Docker Compose

When using docker-compose to run Mware Application Development Framework you will have a container named 'bbproxy' that uses Nginx to proxy requests into the app containers. The Nginx `Dockerfile` adds a script `80-listen-on-ipv6-by-default.sh`([source](https://github.com/Budibase/budibase/blob/master/hosting/proxy/80-listen-on-ipv6-by-default.sh)) to `/docker-entrypoint.d`. This script checks for IPv6 support and if not available removes any IPv6 lines in the Nginx config files.
