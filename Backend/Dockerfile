FROM node:20-alpine

WORKDIR /api
COPY ./package.json .
RUN ["npm", "i", "--no-audit"]

COPY ./dist ./dist

CMD ["npm" ,"start"]