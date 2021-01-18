FROM php:7.4-alpine

RUN apk add --no-cache supervisor

RUN addgroup -g 1000 worker \
    && adduser -D -G worker -u 1000 worker \
    && addgroup www-data users \
    && addgroup worker www-data

COPY supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]

WORKDIR /etc/supervisor/conf.d/
