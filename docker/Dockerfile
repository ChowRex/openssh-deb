ARG DISTRO=ubuntu
ARG DISTVER=22.04
FROM $DISTRO:$DISTVER
WORKDIR /data
# Copy all files
COPY . /data
ARG APT_MIRROR=""
ENV DEBIAN_FRONTEND=noninteractive

RUN if [ ! -z "$APT_MIRROR" ]; then \
        if [ -f /etc/apt/sources.list ]; then \
            sed -i "s|$(awk '/^deb/{print $2}' /etc/apt/sources.list | head -n1 | cut -d/ -f3)|${APT_MIRROR}|" /etc/apt/sources.list; \
        fi; \
        if [ -f /etc/apt/sources.list.d/debian.sources ]; then \
            sed -i "s|deb.debian.org|${APT_MIRROR}|" /etc/apt/sources.list.d/debian.sources;  \
        fi; \
    fi && \
    apt update && apt upgrade -y && \
    apt install -y lsb-release && \
    if [ $(lsb_release -sc) != "focal" ]; then \
        apt install -y dh-virtualenv; \
    fi && \
    apt install -y wget sudo pkgconf build-essential fakeroot dpkg-dev debhelper debhelper-compat dh-exec dh-runit libaudit-dev libedit-dev libgtk-3-dev libselinux1-dev libsystemd-dev libkrb5-dev libpam0g-dev libwrap0-dev && \
    if [ $(apt-cache search --names-only 'libfido2-dev' | wc -l) -gt 0 ]; then \
        apt install -y libfido2-dev libcbor-dev; \
    fi && \
    apt clean all && \
    bash /data/pullsrc.sh

CMD ["bash", "/data/compile.sh"]
