FROM node:20-alpine

WORKDIR /app

COPY payment-service/package.json .
COPY payment-service/tsconfig.json .
COPY payment-service/src ./src

RUN npm install
RUN npm run build

EXPOSE 3005

CMD ["npm", "start"]
