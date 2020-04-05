########################################
FROM maven:slim as build

RUN apt-get update && apt-get -y install git cmake libtool g++

RUN git clone --recurse-submodules https://github.com/mstyura/jitsi-sctp

RUN cd jitsi-sctp && \
    mvn clean package install -Pbuild-usrsctp -Pbuild-jnisctp -f pom.xml

########################################

ARG JITSI_REPO=jitsi
#FROM ${JITSI_REPO}/base-java
FROM jitsi/base-java

RUN \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jitsi-videobridge2 && \
	apt-cleanup

COPY --from=build /jitsi-sctp/jniwrapper/native/target/jniwrapper-native-1.0-SNAPSHOT-linux.jar /usr/share/jitsi-videobridge/lib/jniwrapper-native-1.0-SNAPSHOT.jar

COPY rootfs/ /

VOLUME /config
