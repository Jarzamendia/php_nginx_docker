FROM alpine:3.12
  
# Instalando o S6
ENV S6_OVERLAY_VERSION "2.0.0.1"
ENV PHP_VERSION=7.3.24-r0
ENV APCU_VERSION=5.1.19-r0
ENV NGINX_PROCESS=1

# Instalando dependencias
RUN apk add --no-cache wget ca-certificates nginx

# Instalando o S6 Overlay
RUN cd /tmp && \
	wget https://github.com/just-containers/s6-overlay/releases/download/v$S6_OVERLAY_VERSION/s6-overlay-amd64.tar.gz && \
	tar xzf s6-overlay-amd64.tar.gz -C / && \
	rm s6-overlay-amd64.tar.gz;

# Criando usuário www-data
RUN set -x ; \
  addgroup -g 82 -S www-data ; \
  adduser -u 82 -D -S -G www-data www-data;

RUN mkdir -p /var/www/html && \
	chown -R www-data:www-data /var/www/html && \
	ln -sf /dev/stdout /var/log/nginx/access.log && \
	ln -sf /dev/stderr /var/log/nginx/error.log;

COPY rootfs/ /etc/
COPY conf/nginx.conf /etc/nginx/nginx.conf
COPY conf/default.conf /etc/nginx/conf.d/default.conf
COPY conf/wait-for.sh /wait-for.sh
EXPOSE 80

VOLUME ["/var/www/html"]

ENTRYPOINT ["/init"]