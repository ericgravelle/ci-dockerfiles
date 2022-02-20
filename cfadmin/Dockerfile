FROM ericgravelle/bosh-cli:latest

RUN set -x\
        && adduser -aG sudo --uid 1000 --shell /bin/bash cfadmin \
        && echo "cfadmin ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/cfadmin \
	&& apt-get update \
        && apt-get install -y ca-certificates curl apt-transport-https lsb-release gnupg software-properties-common `\
        && curl -sL https://packages.microsoft.com/keys/microsoft.asc | \
		gpg --dearmor | \
		sudo tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null \
        && echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/$(lsb_release -cs) main" | \
		 sudo tee /etc/apt/sources.list.d/azure-cli.list \
        && apt-add-repository -y ppa:deadsnakes/ppa \
        && apt-get update \
        && apt-get upgrade -y \
	&& apt-get install -y ruby-full python3 git openssh-client dnsutils nmap openssl curl apt-utils \
		azure-cli awscli openjdk-11-jre-headless python3-pip python3-venv \
        && gem -v --norc install cf-uaac \
	&& rm -rf /var/lib/apt/lists/*

COPY credhub-cli/credhub /usr/local/bin/credhub

RUN set -x\
	&& chown root:root /usr/local/bin/credhub \
	&& chmod 555 /usr/local/bin/credhub

USER cfadmin
