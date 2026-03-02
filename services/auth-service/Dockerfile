FROM node:25.7.0-alpine
LABEL maintainer=MorganEtWilliam
WORKDIR /app

COPY package*.json .

RUN npm install

EXPOSE 3001

CMD ["npm", "run", "dev"]