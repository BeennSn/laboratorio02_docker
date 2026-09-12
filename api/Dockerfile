FROM node:22-alpine

RUN apk --no-cache add tini
ENTRYPOINT ["/sbin/tini", "--"]

WORKDIR /opt
ENV NODE_PATH=/opt

COPY package.json package-lock.json ./
RUN npm ci --omit=dev

COPY src ./src

EXPOSE 3000

CMD ["npm", "start"]