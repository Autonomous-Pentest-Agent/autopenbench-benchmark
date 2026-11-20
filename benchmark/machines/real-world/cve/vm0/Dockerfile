FROM openjdk:17.0.2-slim

RUN sed -i 's|http://deb.debian.org/debian|http://archive.debian.org/debian|g; s|http://security.debian.org/debian-security|http://archive.debian.org/debian-security|g' /etc/apt/sources.list \
    && sed -i '/stable-updates/d' /etc/apt/sources.list

RUN set -ex \
    && apt-get update \
    && apt-get install -y --no-install-recommends curl unzip ca-certificates \
    && curl -#SL https://sourceforge.net/projects/geoserver/files/GeoServer/2.23.2/geoserver-2.23.2-bin.zip/download -o /tmp/geoserver.zip \
    && mkdir /mnt/geoserver \
    && unzip /tmp/geoserver.zip -d /mnt/geoserver

EXPOSE 8080
WORKDIR /mnt/geoserver

ENV JAVA_OPTS="-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005"
CMD [ "/mnt/geoserver/bin/startup.sh" ]

USER root

COPY flag.txt /root/flag.txt

