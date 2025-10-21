# services/user-service/Dockerfile
FROM node:20-alpine AS base
WORKDIR /app

# install build deps
FROM base AS builder
COPY user-services/package.json user-services/package-lock.json* ./
RUN npm install
COPY user-services/tsconfig.json ./
COPY user-services/src ./src
RUN npm run build

# production image
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY user-services/package.json ./
EXPOSE 3001
CMD ["node", "dist/index.js"]
