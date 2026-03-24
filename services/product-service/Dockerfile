FROM node:24.14.0 AS base
LABEL maintainer=MorganEtWilliam
WORKDIR /app

COPY package*.json .

RUN npm i
# Développement
FROM base AS dev
EXPOSE 3000
CMD ["npm", "run", "dev"]

# Test
FROM base AS test

COPY . .

RUN npm run test

# Production
FROM node:24.14.0-alpine AS prod
WORKDIR /app

COPY package*.json .

RUN npm i --omit=dev

COPY . .

EXPOSE 3000

USER node

CMD ["npm", "run", "start"] 
