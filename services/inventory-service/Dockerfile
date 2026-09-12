
# Build stage
# =========================
FROM node:22-alpine AS builder

WORKDIR /app

# Copy dependency files first for Docker layer caching
COPY package*.json ./

RUN npm ci --omit=dev

# Copy application source
COPY src ./src


# =========================
# Runtime stage
# =========================
FROM node:22-alpine

WORKDIR /app

ENV NODE_ENV=production

COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/src ./src
COPY --from=builder /app/package.json ./package.json

EXPOSE 8083

USER node

CMD ["node", "src/index.js"]
