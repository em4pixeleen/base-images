FROM resin/i386-alpine:3.3

LABEL io.resin.device-type="edison"

RUN apk add --update \
		less \
		nano \
		net-tools \
		ifupdown \		
		usbutils \
		gnupg \
	&& rm -rf /var/cache/apk/*
