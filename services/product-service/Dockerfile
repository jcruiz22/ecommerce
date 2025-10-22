# services/product-service/Dockerfile
FROM node:20-alpine AS base
WORKDIR /app

# install build deps
FROM base AS builder
COPY product-service/package.json product-service/package-lock.json* ./
RUN npm install
COPY product-service/tsconfig.json ./
COPY product-service/src ./src
RUN npm run build

# production image
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY product-service/package.json ./
EXPOSE 3002
CMD ["node", "dist/index.js"]