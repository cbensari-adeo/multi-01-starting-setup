FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

ENV MONGODB_USERNAME=****
ENV MONGODB_PASSWORD=****

EXPOSE 80

CMD [ "npm", "start"]
