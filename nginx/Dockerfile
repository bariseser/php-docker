FROM nginx:alpine

LABEL maintainer="Baris Eser <bariseser@gmail.com>"

RUN rm /etc/nginx/nginx.conf
COPY nginx.conf /etc/nginx/

RUN apk update \
    && apk upgrade \
    && apk add --no-cache openssl \
    && apk add --no-cache bash

RUN set -x ; \
    addgroup -g 82 -S www-data ; \
    adduser -u 82 -D -S -G www-data www-data && exit 0 ; exit 1

RUN rm -rf /etc/nginx/conf.d/default.conf

RUN touch /var/log/nginx

ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash", "/opt/startup.sh"]

EXPOSE 80 443

ENTRYPOINT nginx -g 'daemon off;'