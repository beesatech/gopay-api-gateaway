# Standalone build of the GoPay Merchant gateway. Persists the GoBiz session on the /session volume
# so a container rebuild does not force a re-login. Build context is the repo root:
#   docker compose -f deploy/docker-compose.yml build
FROM node:20-alpine
RUN apk add --no-cache curl
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev && npm cache clean --force
COPY . .
RUN mkdir -p /session \
 && ln -sf /session/.GOPAY_SESI_JANGAN_DIHAPUS.json /app/.GOPAY_SESI_JANGAN_DIHAPUS.json \
 && ln -sf /session/.gopay_cache.json /app/.gopay_cache.json
ENV PORT=3000
EXPOSE 3000
HEALTHCHECK --interval=15s --timeout=5s --start-period=10s --retries=3 CMD curl -fsS "http://localhost:${PORT:-3000}/health" || exit 1
CMD ["node", "server.js"]
