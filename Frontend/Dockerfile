FROM node:20-alpine
WORKDIR /frontend
RUN ["npm","install", "serve", "-g"]

COPY ./dist/index.html ./
COPY ./dist/vite.svg ./
COPY ./dist/assets/index-*.js ./assets/
COPY ./dist/assets/index-*.css ./assets/
COPY ./dist/assets/*.webp ./assets/
COPY ./dist/assets/*.svg ./assets/

CMD ["serve" ,"."]
