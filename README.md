# Tailscale DERP Server

Since the original repository is unmaintained, I forked it and optimise it

## Environment

| Env                   | Default                              | Description                                                                                                                       |
| --------------------- | ------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| `DERP_DOMAIN`         | `derp01.tailscale.com`               |                                                                                                                                   |
| `DERP_CERT_MODE`      | `letsencrypt`                        |                                                                                                                                   |
| `DERP_CERT_DIR`       | `/app/certs`                         |                                                                                                                                   |
| `DERP_ADDR`           | `:443`                               | If you need to use a reverse proxy (Nginx, Traefik, Caddy) to manage TLS certificates, you can set ":80" to listen for http.      |
| `DERP_STUN`           | `true`                               | Enable STUN server                                                                                                                |
| `DERP_STUN_PORT`      | `3478`                               |                                                                                                                                   |
| `DERP_HTTP_PORT`      | `80`                                 |                                                                                                                                   |
| `DERP_VERIFY_CLIENTS` | `true`                               |                                                                                                                                   |
| `TS_SERVER`           | `https://controlplane.tailscale.com` | If you are using self-hosted `headscale`, set the server address. Only available `DERP_VERIFY_CLIENTS=true`.                      |
| `TS_AUTHKEY`          | `tskey-abcdef1234567890`             | If you only allow verify-client, you **MUST** set the auth key. (<https://tailscale.com/kb/1085/auth-keys>)                       |
| `TS_EXTRA_ARGS`       |                                      | Tailscale CLI arguments, for example you can set the node name via `--hostname=derp-01`. (<https://tailscale.com/kb/1080/cli#up>) |

## Example

### Docker Compose + Externally managed certificate

```yaml
services:
  derper:
    image: lee2001s/tailscale-derper:latest
    restart: always
    ports:
      - 3478:3478/udp
      - 127.0.0.1:23333:80
    environment:
      - DERP_DOMAIN=derp-jp.leeteng.com
      - DERP_ADDR=:80
      - TS_AUTHKEY=tskey-auxxxxx
    volumes:
      - ./tailscale_data:/var/lib/tailscale
```
