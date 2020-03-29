FROM node:12.16.1-alpine

WORKDIR /app

COPY package.json yarn.lock tsconfig.json /app/
RUN yarn install

COPY . /app/

CMD ["yarn", "start"]
