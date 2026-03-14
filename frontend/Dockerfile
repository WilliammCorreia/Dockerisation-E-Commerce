FROM node:24.14.0 AS base
LABEL maintainer=MorganEtWilliam
WORKDIR /app

COPY package*.json .

RUN npm install

# Développement
FROM base AS dev

EXPOSE 8080

CMD ["npm", "run", "dev"]

# Test

# Build
FROM base AS build

COPY . .

RUN npm run build

# Production
FROM nginx:1.28.2-alpine3.23-slim AS prod

RUN chown -R nginx:nginx /var/cache/nginx /var/run /var/log/nginx /run

COPY --from=build /app/dist /usr/share/nginx/html

COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 8080

USER nginx