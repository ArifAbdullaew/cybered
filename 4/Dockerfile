FROM debian:bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive

ARG USERNAME=arif
ARG GROUPNAME=abdullaev
ARG SITENAME=cybered.ru

RUN set -eux; \
    mkdir -p /etc/apt/sources.list.d; \
    printf "deb http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware\n" > /etc/apt/sources.list.d/cybered.list; \
    printf "deb http://deb.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware\n" >> /etc/apt/sources.list.d/cybered.list; \
    printf "deb http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware\n" >> /etc/apt/sources.list.d/cybered.list; \
    apt-get update; \
    apt-get -y upgrade; \
    apt-get -y install --no-install-recommends nginx ca-certificates; \
    rm -rf /var/lib/apt/lists/*

RUN set -eux; \
    rm -rf /var/www/*; \
    mkdir -p "/var/www/${SITENAME}/image"

COPY index.html "/var/www/${SITENAME}/index.html"
COPY image.jpg "/var/www/${SITENAME}/image/image.jpg"

RUN set -eux; \
    chmod -R 754 "/var/www/${SITENAME}"

RUN set -eux; \
    groupadd -f "${GROUPNAME}"; \
    useradd -m -s /bin/bash -U "${USERNAME}"; \
    usermod -aG "${GROUPNAME}" "${USERNAME}"; \
    chown -R "${USERNAME}:${GROUPNAME}" "/var/www/${SITENAME}"

RUN set -eux; \
    sed -i "s|/var/www/html|/var/www/${SITENAME}|g" /etc/nginx/sites-available/default

RUN set -eux; \
    sed -i "s|^user .*;|user ${USERNAME} ${GROUPNAME};|g" /etc/nginx/nginx.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]