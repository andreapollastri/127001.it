# 127001.it

> Free wildcard DNS for local development. Stop editing your hosts file.

## What is it?

`*.127001.it` is a public wildcard DNS record that resolves every subdomain to `127.0.0.1`. That's it. No installation, no account, no configuration required.

## Why use it?

Testing subdomain-based features locally (multi-tenancy, split frontends, OAuth flows, API gateways) normally means editing `/etc/hosts` every time. With 127001.it you just use the URL directly — it works out of the box for everyone on the team, on any OS.

## Usage

Replace `localhost` with any `*.127001.it` subdomain in your app config:

```
# .env
APP_URL=http://myapp.127001.it:8000
TENANT_A=http://tenant-a.127001.it:8000
TENANT_B=http://tenant-b.127001.it:8000
```

Combine with any port. No hosts file entry needed.

## Local HTTPS with Caddy

Add a `Caddyfile` and a Docker service to get a trusted SSL certificate locally:

```
# Caddyfile
myapp.127001.it {
  reverse_proxy app:80
  tls internal
}
```

```yaml
# docker-compose.yml
services:
  caddy:
    image: caddy:latest
    ports:
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - caddy_data:/data
      - caddy_config:/config
    networks:
      - app_network
```

Then trust the local CA once:

```bash
docker compose exec caddy caddy trust
```

## Website

[127001.it](https://127001.it)

## Author

[Andrea Pollastri](https://web.ap.it)

## License

Free to use.
