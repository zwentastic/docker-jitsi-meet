ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base-java

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jicofo && \
	apt-cleanup

COPY rootfs/ /

RUN \
	sed -i "s/Xmx3072m/Xmx1024m/g" /usr/share/jicofo/jicofo.sh

VOLUME /config

