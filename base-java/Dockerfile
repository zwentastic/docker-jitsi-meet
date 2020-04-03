ARG JITSI_REPO=jitsi
FROM ${JITSI_REPO}/base

RUN	\
	mkdir -p /usr/share/man/man1 && \
	apt-dpkg-wrap apt-get update && \
	#apt-dpkg-wrap apt-get install -y openjdk-8-jre-headless && \
	apt-dpkg-wrap apt-get install -y openjdk-8-jdk maven && \
	apt-cleanup

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-armhf
