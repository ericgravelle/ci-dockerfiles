FROM ericgrav/bosh-cli:latest

USER root 

RUN set -x\
	&& curl -sL https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | apt-key add - \
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
        && apt-get install -y ruby-full ruby-dev \
	&& rm -rf /var/lib/apt/lists/* 

COPY --chown=bin credhub-cli/credhub /usr/local/bin/credhub

RUN set -x\
	&& chmod 555 /usr/local/bin/credhub


USER cfadmin

RUN set -x\
        && gem install cf-uaac 
