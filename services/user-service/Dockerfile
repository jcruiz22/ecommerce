# services/user-service/Dockerfile
FROM node:20-alpine AS base
WORKDIR /app

# install build deps
FROM base AS builder
COPY user-service/package.json user-service/package-lock.json* ./
RUN npm install
COPY user-service/tsconfig.json ./
COPY user-service/src ./src
RUN npm run build

# production image
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY user-service/package.json ./
EXPOSE 3001
CMD ["node", "dist/index.js"]
