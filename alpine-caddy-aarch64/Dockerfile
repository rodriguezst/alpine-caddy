FROM yobasystems/alpine:3.8.1-aarch64
LABEL maintainer "Dominic Taylor <dominic@yobasystems.co.uk>" architecture="ARM64v8/aarch64" alpineversion="3.8.1" caddy-version="0.11.1" build="29-nov-2018"

ARG plugins=http.git,http.cache,http.expires,http.minify,http.realip

RUN apk add --no-cache openssh-client git tar curl libcap

RUN curl --silent --show-error --fail --location --header "Accept: application/tar+gzip, application/x-gzip, application/octet-stream" -o - \
      "https://caddyserver.com/download/linux/arm64?plugins=${plugins}&license=personal&telemetry=off" \
    | tar --no-same-owner -C /usr/bin/ -xz caddy && \
    chmod 0755 /usr/bin/caddy && \
    addgroup -S caddy && \
    adduser -D -S -s /sbin/nologin -G caddy caddy && \
    setcap cap_net_bind_service=+ep `readlink -f /usr/bin/caddy` && \
    /usr/bin/caddy -version

EXPOSE 80 443 2015
VOLUME /srv
WORKDIR /srv

ADD files/Caddyfile /etc/Caddyfile
ADD files/index.html /srv/index.html

RUN chown -R caddy:caddy /srv

USER caddy

ENTRYPOINT ["/usr/bin/caddy"]
CMD ["--conf", "/etc/Caddyfile"]
