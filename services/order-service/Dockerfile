# services/order-service/Dockerfile
FROM node:20-alpine AS base
WORKDIR /app

# install build deps
FROM base AS builder
COPY order-service/package.json oreder-service/package-lock.json* ./
RUN npm install
COPY order-service/tsconfig.json ./
COPY order-service/src ./src
RUN npm run build

# production image
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY order-service/package.json ./
EXPOSE 3002
CMD ["node", "dist/index.js"]