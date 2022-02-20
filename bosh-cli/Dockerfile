FROM ubuntu:focal

USER root

RUN set -x\
        && adduser --home /cfadmin --shell /bin/bash --uid 1000 --disabled-password --gecos "CF Admin" cfadmin \
        && usermod -G sudo cfadmin \
        && mkdir -p /etc/sudoers.d \
        && echo "cfadmin ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/cfadmin

RUN set -x\
        && apt-get update \
        && apt-get upgrade -y \
	&& rm -rf /var/lib/apt/lists/*

RUN set -x\
        && apt-get update \
        && ln -fs /usr/share/zoneinfo/UTC /etc/localtime \
        && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata \
        && dpkg-reconfigure --frontend noninteractive tzdata \
        && rm -rf /var/lib/apt/lists/*

RUN set -x\
	&& apt-get update \
        && apt-get upgrade -y \
	&& apt-get install -y git ca-certificates curl apt-transport-https sudo vim apt-utils azure-cli awscli \
	&& rm -rf /var/lib/apt/lists/*

COPY --chown=bin bosh-cli/bosh /usr/local/bin/bosh

RUN set -x\
	&& chmod 555 /usr/local/bin/bosh

USER cfadmin
