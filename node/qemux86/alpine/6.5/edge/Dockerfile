# AUTOGENERATED FILE
FROM resin/qemux86-alpine-buildpack-deps:edge

ENV NODE_VERSION 6.5.0

# Install dependencies
RUN apk add --no-cache libgcc libstdc++ libuv

RUN curl -SLO "http://resin-packages.s3.amazonaws.com/node/v$NODE_VERSION/node-v$NODE_VERSION-linux-alpine-i386.tar.gz" \
	&& echo "c7c26de9bdc4555e4b7495b8a82ba59660fcea2ce732f663c4b03cc46d800bc9  node-v6.5.0-linux-alpine-i386.tar.gz" | sha256sum -c - \
	&& tar -xzf "node-v$NODE_VERSION-linux-alpine-i386.tar.gz" -C /usr/local --strip-components=1 \
	&& rm "node-v$NODE_VERSION-linux-alpine-i386.tar.gz" \
	&& npm config set unsafe-perm true -g --unsafe-perm \
	&& rm -rf /tmp/*

CMD ["echo","'No CMD command was set in Dockerfile! Details about CMD command could be found in Dockerfile Guide section in our Docs. Here's the link: http://docs.resin.io/deployment/dockerfile"]
