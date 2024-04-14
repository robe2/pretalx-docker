# NGINX Config example

Change the port section of the web container to below, note the 8355 port
as that is the port that will be exposed for proxy.
You can use a different port other than 8355,
but make sure the proxy_pass line matches whatever port you choose.

```
    ports:
      - "8355:80"
```
Your config could look like this:

```
server {
    listen 80 default_server;
    listen [::]:80 ipv6only=on default_server;
    server_name pretalx.mydomain.com;
}
server {
    listen 443 default_server;
    listen [::]:443 ipv6only=on default_server;
    server_name pretalx.mydomain.com;

    ssl on;
    ssl_certificate /path/to/cert.chain.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://localhost:8355/;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header Host $http_host;
    }
}
```
