# AUTOGENERATED FILE
FROM resin/edison-buildpack-deps:wheezy

ENV NODE_VERSION 0.12.15

RUN curl -SLO "http://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x86.tar.gz" \
	&& echo "0ea7a2860311596a13e202fef507ec2a55db9ddbb1d9cb27fa19b7e61d56ee20  node-v0.12.15-linux-x86.tar.gz" | sha256sum -c - \
	&& tar -xzf "node-v$NODE_VERSION-linux-x86.tar.gz" -C /usr/local --strip-components=1 \
	&& rm "node-v$NODE_VERSION-linux-x86.tar.gz" \
	&& npm install mraa@$MRAA_VERSION \
	&& npm cache clear \
	&& npm config set unsafe-perm true -g --unsafe-perm \
	&& rm -rf /tmp/*

CMD ["echo","'No CMD command was set in Dockerfile! Details about CMD command could be found in Dockerfile Guide section in our Docs. Here's the link: http://docs.resin.io/deployment/dockerfile"]
