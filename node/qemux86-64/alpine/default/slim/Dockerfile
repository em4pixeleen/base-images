# AUTOGENERATED FILE
FROM resin/qemux86-64-alpine:latest

ENV NODE_VERSION 0.10.22

# Install dependencies
RUN apk add --no-cache libgcc libstdc++ libuv

RUN buildDeps='curl' \
	&& set -x \
	&& apk add --no-cache $buildDeps \
	&& curl -SLO "http://resin-packages.s3.amazonaws.com/node/v$NODE_VERSION/node-v$NODE_VERSION-linux-alpine-amd64.tar.gz" \
	&& echo "6df90b3a9e4a6954ac4234838f98943fbd4779d0160dce8231bbd13dab155374  node-v0.10.22-linux-alpine-amd64.tar.gz" | sha256sum -c - \
	&& tar -xzf "node-v$NODE_VERSION-linux-alpine-amd64.tar.gz" -C /usr/local --strip-components=1 \
	&& rm "node-v$NODE_VERSION-linux-alpine-amd64.tar.gz" \
	&& apk del $buildDeps \
	&& npm config set unsafe-perm true -g --unsafe-perm \
	&& rm -rf /tmp/*

CMD ["echo","'No CMD command was set in Dockerfile! Details about CMD command could be found in Dockerfile Guide section in our Docs. Here's the link: http://docs.resin.io/deployment/dockerfile"]
