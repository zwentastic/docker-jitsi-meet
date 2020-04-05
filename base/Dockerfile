 
###############################
FROM golang:1.12-alpine AS build

RUN apk add --no-cache make git

RUN mkdir -p /go/src/github.com/subchen/frep
RUN git clone https://github.com/subchen/frep.git /go/src/github.com/subchen/frep
WORKDIR /go/src/github.com/subchen/frep

RUN GO111MODULE=on CGO_ENABLED=0 GOOS=linux GOARCH=arm go build -a -ldflags "-s -w" -installsuffix cgo -o _releases/frep-1.3.8

###############################

FROM debian:stretch-slim

ARG JITSI_RELEASE=stable

ENV S6_BEHAVIOUR_IF_STAGE2_FAILS=2

ADD https://github.com/just-containers/s6-overlay/releases/download/v1.22.1.0/s6-overlay-armhf.tar.gz /tmp/s6-overlay.tar.gz
ADD https://download.jitsi.org/jitsi-key.gpg.key /tmp/jitsi.key
COPY --from=build /go/src/github.com/subchen/frep/_releases/frep-* /usr//bin/frep

COPY rootfs /

RUN \
	tar xfz /tmp/s6-overlay.tar.gz -C / && \
	rm -f /tmp/*.tar.gz && \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y apt-transport-https apt-utils ca-certificates gnupg && \
	apt-key add /tmp/jitsi.key && \
	rm -f /tmp/jitsi.key && \
	echo "deb https://download.jitsi.org $JITSI_RELEASE/" > /etc/apt/sources.list.d/jitsi.list && \
	echo "deb http://ftp.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/backports.list && \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get dist-upgrade -y && \
	apt-cleanup && \
	chmod +x /usr/bin/frep

RUN \
	[ "$JITSI_RELEASE" = "unstable" ] && \
	apt-dpkg-wrap apt-get update && \
	apt-dpkg-wrap apt-get install -y jq procps curl vim iputils-ping net-tools && \
	apt-cleanup || \
	true

ENTRYPOINT [ "/init" ]
