FROM node:21-slim

ENV PORT 3000

EXPOSE ${PORT}

WORKDIR /app

ADD ./package.json ./package.json

ADD ./package-lock.json ./package-lock.json

RUN npm install --force

ADD . /app

RUN npm run build

ENV NODE_ENV production

CMD ["npm", "start"]