FROM node:20-alpine
FROM postgres:15-alpine

WORKDIR /app

COPY package.json .
COPY app.js .
CMD ["node", "app.js"]
