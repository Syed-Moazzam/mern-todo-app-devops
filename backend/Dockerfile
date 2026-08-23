FROM node:21-slim

ENV PORT 5000

EXPOSE ${PORT}

WORKDIR /app

ADD ./package.json ./package.json

ADD ./package-lock.json ./package-lock.json

RUN npm ci

ADD . /app

ENV NODE_ENV production

CMD ["npm", "run", "dev"]