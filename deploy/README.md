# Deploying the gateway as a standalone service

This gateway runs on its own, independent of any consumer app, so several applications can share it
(they all read the one GoPay merchant account its session belongs to).

```bash
# once: a shared network consumers also join
docker network create gopay-net

cp deploy/.env.example deploy/.env    # set API_KEY and DOMAIN
docker compose -f deploy/docker-compose.yml up -d --build

# one-time GoBiz OTP login (persists on the gopay_session volume)
docker compose -f deploy/docker-compose.yml exec gopay node login.js
```

Consumers call it with the `x-api-key: <API_KEY>` header, either internally on `gopay-net`
(`http://<STACK>_gateway:3000`, default `http://gopay_gateway:3000`) or publicly at `https://<DOMAIN>`.
Add `DOMAIN` to your Cloudflare tunnel / DNS for public access. One instance = one GoPay account;
run another instance (different `STACK`, `DOMAIN`, `SESSION_VOLUME`, `API_KEY`) for another account.
