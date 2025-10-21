# services/cart-service/Dockerfile
FROM node:20-alpine AS base
WORKDIR /app

# install build deps
FROM base AS builder
COPY cart-services/package.json cart-services/package-lock.json* ./
RUN npm install
COPY cart-services/tsconfig.json ./
COPY cart-services/src ./src
RUN npm run build

# production image
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY cart-services/package.json ./
EXPOSE 3003
CMD ["node", "dist/index.js"]