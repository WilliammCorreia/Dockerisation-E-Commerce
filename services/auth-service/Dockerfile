FROM node:25.7.0-alpine
LABEL maintainer=MorganEtWilliam
WORKDIR /app

COPY package*.json .

RUN npm install

COPY . .

EXPOSE 3001

CMD ["node", "src/app.js"]