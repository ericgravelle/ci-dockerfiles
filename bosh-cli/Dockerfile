FROM ubuntu:focal

RUN set -x\
	&& apt-get update \
        && apt-get upgrade -y \
	&& apt-get install -y ruby-full ruby-dev git \
        && apt-get autoremove -y wget \
	&& rm -rf /var/lib/apt/lists/*

COPY bosh-cli/bosh /usr/local/bin/bosh

RUN set -x\
	&& chown root:root /usr/local/bin/bosh \
	&& chmod 555 /usr/local/bin/bosh
