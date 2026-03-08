FROM node:24.14.0 AS base
LABEL maintainer=MorganEtWilliam
WORKDIR /app

COPY package*.json .

RUN npm install

# Développement
FROM base AS dev

EXPOSE 3001

CMD ["npm", "run", "dev"]

# Test
FROM base AS test

COPY . .

RUN npm run test

# Production
FROM node:24.14.0-alpine AS prod
WORKDIR /app

COPY package*.json .

RUN npm ci --omit=dev

COPY . .

EXPOSE 3001

CMD ["npm", "run", "start"]