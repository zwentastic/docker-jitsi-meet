ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base-java

RUN \
	apt-dpkg-wrap apt-get update && \
	#apt-dpkg-wrap apt-get install -y jitsi-videobridge2 wget unzip && \
	apt-dpkg-wrap apt-get install -y wget unzip && \
	cd /tmp && \
	wget http://download.jitsi.org/jitsi-videobridge/linux/jitsi-videobridge-linux-x64-1132.zip && \
	unzip jitsi-videobridge-linux-x64-1132.zip && \
	mv /tmp/jitsi-videobridge-linux-x64-1132 /usr/share/jitsi-videobridge && \
	groupadd --gid 1000 jitsi && \
	useradd --create-home --shell /bin/bash --uid 1000 --gid 1000 jvb && \
	apt-cleanup


COPY rootfs/ /

VOLUME /config
