FROM debian:buster
LABEL maintainer "w00412055"

ENV HOME="/" \
    OS_ARCH="arm64" \
    OS_FLAVOUR="debian-10" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages acl ca-certificates curl gzip libc6 libgeoip1 libpcre3 libssl1.1 procps tar zlib1g
COPY gosu-1.12.0-2-linux-arm64-debian-10.tar.gz /tmp/bitnami/pkg/cache/
COPY nginx-1.19.9-0-linux-arm64-debian-10.tar.gz /tmp/bitnami/pkg/cache/
COPY render-template-1.0.0-3-linux-arm64-debian-10.tar.gz /tmp/bitnami/pkg/cache/

RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "render-template" "1.0.0-3" 
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "nginx" "1.19.9-0" 
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.12.0-2" 
RUN chmod g+rwX /opt/bitnami
RUN ln -sf /dev/stdout /opt/bitnami/nginx/logs/access.log
RUN ln -sf /dev/stderr /opt/bitnami/nginx/logs/error.log

COPY rootfs /
RUN /opt/bitnami/scripts/nginx/postunpack.sh
ENV BITNAMI_APP_NAME="nginx" \
    BITNAMI_IMAGE_VERSION="1.19.8-debian-10-r14" \
    NGINX_ENABLE_CUSTOM_PORTS="no" \
    NGINX_HTTPS_PORT_NUMBER="" \
    NGINX_HTTP_PORT_NUMBER="" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/nginx/sbin:$PATH"

EXPOSE 8080 8443

WORKDIR /app
USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/nginx/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/nginx/run.sh" ]
