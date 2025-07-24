FROM nginx:1.29.0-alpine-slim
COPY ./public /usr/share/nginx/html
COPY .build/nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80