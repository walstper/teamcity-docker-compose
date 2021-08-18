FROM jetbrains/teamcity-minimal-agent:2021.1.2

USER root

ARG DockerComposeVersion=1.29.2
ARG NodeVersion=14

# Install basic tools
RUN apt-get remove docker docker-engine docker.io containerd runc && \
    apt-get update && \
    apt-get -y --no-install-recommends install \
	dirmngr \
	curl \
	gpg-agent \
	ca-certificates \
	software-properties-common \
	apt-transport-https \
	wget \
	zip \
	git \
	locales

# Set the locale
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# docker compose
RUN curl -L "https://github.com/docker/compose/releases/download/${DockerComposeVersion}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose
RUN docker-compose --version

# install node and yarn 
RUN curl -sL "https://deb.nodesource.com/setup_${NodeVersion}.x" -o nodesource_setup.sh
RUN bash nodesource_setup.sh
RUN apt-get install -y nodejs
RUN node -v
RUN npm install -g npm
RUN npm -v

# add docker.io repo and install it
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && apt-key fingerprint 0EBFCD88
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

RUN apt-get update && \
    apt-get -y install docker-ce docker-ce-cli containerd.io