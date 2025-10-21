# services/product-service/Dockerfile
FROM node:20-alpine AS base
WORKDIR /app

# install build deps
FROM base AS builder
COPY product-services/package.json product-services/package-lock.json* ./
RUN npm install
COPY product-services/tsconfig.json ./
COPY product-services/src ./src
RUN npm run build

# production image
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY product-services/package.json ./
EXPOSE 3002
CMD ["node", "dist/index.js"]