FROM resin/artik10-alpine-buildpack-deps:edge

# remove several traces of python
RUN apk del python*

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8

# install dbus-python dependencies 
RUN apk add --no-cache \
		dbus-dev \
		dbus-glib-dev

# key 63C7CC90: public key "Simon McVittie <smcv@pseudorandom.co.uk>" imported
# key 3372DCFA: public key "Donald Stufft (dstufft) <donald@stufft.io>" imported
RUN gpg --keyserver keyring.debian.org --recv-keys 4DE8FF2A63C7CC90 \
	&& gpg --keyserver pgp.mit.edu  --recv-key 6E3CBCE93372DCFA \
	&& gpg --keyserver pgp.mit.edu --recv-keys 0x52a43a1e4b77b059

ENV PYTHON_VERSION 2.7.12

# if this is called "PIP_VERSION", pip explodes with "ValueError: invalid truth value '<VERSION>'"
ENV PYTHON_PIP_VERSION 8.1.2
ENV PYTHON_PIP_SHA256 8dae1fb72e29c2b6ff6ed267861179216bf98d3bda6d30e527dbed0db5ac7e1d

ENV SETUPTOOLS_SHA256 76898498d835be860d82d3c37b1c8393aa0c4100ca35f68ecb895688e7aed6c8
ENV SETUPTOOLS_VERSION 25.1.1

# point Python at a system-provided certificate database. Otherwise, we might hit CERTIFICATE_VERIFY_FAILED.
# https://www.python.org/dev/peps/pep-0476/#trust-database
ENV SSL_CERT_FILE /etc/ssl/certs/ca-certificates.crt

RUN set -x \
	&& curl -SLO "http://resin-packages.s3.amazonaws.com/python/v$PYTHON_VERSION/Python-$PYTHON_VERSION.linux-alpine-armhf.tar.gz" \
	&& echo "b20c026797d335d1843b062f4033406f7bdfaac4a073d7eb4889adc7dfe37821  Python-2.7.12.linux-alpine-armhf.tar.gz" | sha256sum -c - \
	&& tar -xzf "Python-$PYTHON_VERSION.linux-alpine-armhf.tar.gz" --strip-components=1 \
	&& rm -rf "Python-$PYTHON_VERSION.linux-alpine-armhf.tar.gz" \
	&& mkdir -p /usr/src/python/setuptools \
	&& curl -SLO https://github.com/pypa/setuptools/archive/v$SETUPTOOLS_VERSION.tar.gz \
	&& echo "$SETUPTOOLS_SHA256  v$SETUPTOOLS_VERSION.tar.gz" > setuptools-$SETUPTOOLS_VERSION.tar.gz.sha256sum \
	&& sha256sum -c setuptools-$SETUPTOOLS_VERSION.tar.gz.sha256sum \
	&& tar -xzC /usr/src/python/setuptools --strip-components=1 -f v$SETUPTOOLS_VERSION.tar.gz \
	&& rm -rf v$SETUPTOOLS_VERSION.tar.gz* \
	&& cd /usr/src/python/setuptools \
	&& python2 bootstrap.py \
	&& python2 easy_install.py . \
	&& mkdir -p /usr/src/python/pip \
	&& curl -SL "https://github.com/pypa/pip/archive/$PYTHON_PIP_VERSION.tar.gz" -o pip.tar.gz \
	&& echo "$PYTHON_PIP_SHA256  pip.tar.gz" > pip.tar.gz.sha256sum \
	&& sha256sum -c pip.tar.gz.sha256sum \
	&& tar -xzC /usr/src/python/pip --strip-components=1 -f pip.tar.gz \
	&& rm pip.tar.gz* \
	&& cd /usr/src/python/pip \
	&& python2 setup.py install \
	&& cd .. \
	&& find /usr/local \
		\( -type d -a -name test -o -name tests \) \
		-o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
		-exec rm -rf '{}' + \
	&& cd / \
	&& rm -rf /usr/src/python ~/.cache

# install "virtualenv", since the vast majority of users of this image will want it
RUN pip install --no-cache-dir virtualenv

ENV PYTHON_DBUS_VERSION 1.2.4

# install dbus-python
RUN set -x \
	&& mkdir -p /usr/src/dbus-python \
	&& curl -SL "http://dbus.freedesktop.org/releases/dbus-python/dbus-python-$PYTHON_DBUS_VERSION.tar.gz" -o dbus-python.tar.gz \
	&& curl -SL "http://dbus.freedesktop.org/releases/dbus-python/dbus-python-$PYTHON_DBUS_VERSION.tar.gz.asc" -o dbus-python.tar.gz.asc \
	&& gpg --verify dbus-python.tar.gz.asc \
	&& tar -xzC /usr/src/dbus-python --strip-components=1 -f dbus-python.tar.gz \
	&& rm dbus-python.tar.gz* \
	&& cd /usr/src/dbus-python \
	&& PYTHON=python2.7 ./configure \
	&& make -j$(nproc) \
	&& make install -j$(nproc) \
	&& cd / \
	&& rm -rf /usr/src/dbus-python

CMD ["echo","'No CMD command was set in Dockerfile! Details about CMD command could be found in Dockerfile Guide section in our Docs. Here's the link: http://docs.resin.io/deployment/dockerfile"]
ENV PYTHONPATH /usr/lib/python2.7/dist-packages:/usr/lib/python2.7/site-packages:$PYTHONPATH
