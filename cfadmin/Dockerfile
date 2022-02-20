FROM ericgrav/bosh-cli:latest

RUN set -x\
	&& adduser --home /cfadmin --shell /bin/bash --uid 1000 --disabled-password --gecos "CF Admin,n/a,n/a,n/a,n/a" cfadmin \
	&& usermod -G sudo cfadmin \
        && mkdir -p /etc/sudoers.d \
	&& echo "cfadmin ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/cfadmin

RUN set -x\
	&& apt-get update \
	&& apt-get install -y apt-utils \
	&& rm -rf /var/lib/apt/lists/* 

RUN set -x\
        && apt-get update \
        && ln -fs /usr/share/zoneinfo/America/New_York /etc/localtime \
        && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata \
        && dpkg-reconfigure --frontend noninteractive tzdata \
	&& rm -rf /var/lib/apt/lists/* 


RUN set -x\
	&& apt-get update \
        && apt-get install -y ca-certificates curl apt-transport-https sudo \
        lsb-release gnupg software-properties-common lsb-core\
	&& rm -rf /var/lib/apt/lists/* 

RUN set -x\
        && curl -sL https://packages.microsoft.com/keys/microsoft.asc | \
	gpg --dearmor | \
	sudo tee /etc/apt/trusted.gpg.d/microsoft.gpg > /dev/null \
        && echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $(lsb_release -cs) main" | \
        sudo tee /etc/apt/sources.list.d/azure-cli.list \
        && apt-get update \
        && apt-get upgrade -y \
	&& apt-get install -y git openssh-client dnsutils nmap openssl azure-cli \
	&& rm -rf /var/lib/apt/lists/* 

RUN set -x\
	&& wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | sudo apt-key add - \
	&& echo "deb https://packages.cloudfoundry.org/debian stable main" | \
	sudo tee /etc/apt/sources.list.d/cloudfoundry-cli.list \
        && apt-get update \
	&& apt-get install -y cf8-cli \
        && su - -c "cf install-plugin -r CF-Community -f 'check-before-deploy'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'conduit'" cfadmin\
        && su - -c "cf install-plugin -r CF-Community -f 'Create-Service-Push'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'cf-restage-all'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'top'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'cf-rolling-restart'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'push-with-vault'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'bg-restage'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'deploy'" cfadmin \
        && su - -c "cf install-plugin -r CF-Community -f 'Usage Report'" cfadmin \
	&& rm -rf /var/lib/apt/lists/* 

RUN set -x\
        && apt-get update \
        && apt-get install -y ruby-full \
        && gem install cf-uaac \
	&& rm -rf /var/lib/apt/lists/* 

COPY credhub-cli/credhub /usr/local/bin/credhub

RUN set -x\
	&& chown root:root /usr/local/bin/credhub \
	&& chmod 555 /usr/local/bin/credhub


USER cfadmin
