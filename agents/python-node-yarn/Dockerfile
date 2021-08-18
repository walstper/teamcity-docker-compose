FROM jetbrains/teamcity-minimal-agent:2021.1.2

USER root

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

ARG PythonVersion=3.9.6
ARG DockerComposeVersion=1.29.2
ARG NodeVersion=14

# install node and yarn
RUN curl -sL "https://deb.nodesource.com/setup_${NodeVersion}.x" -o nodesource_setup.sh
RUN bash nodesource_setup.sh

RUN apt-get update && \
    apt-get install -y nodejs && \
    npm install -g npm yarn 
RUN node -v
RUN npm -v
RUN yarn -v

# Install Python stuff
RUN apt-get install -y \
    build-essential \
    checkinstall \
    libreadline-gplv2-dev \
    libncursesw5-dev \
    libssl-dev \
    libsqlite3-dev \
    tk-dev \
    libgdbm-dev \
    libc6-dev \
    libbz2-dev \
    wget \
    locales

# Set the locale
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install Python
RUN cd /usr/src && wget https://www.python.org/ftp/python/${PythonVersion}/Python-${PythonVersion}.tgz && tar xzf Python-${PythonVersion}.tgz
RUN cd /usr/src/Python-${PythonVersion} && ./configure && make altinstall

RUN apt-get update && apt-get install -y \
    git \
    # python stuff
    python3-pip \
    software-properties-common \
    # pg
    libpq-dev \
    # images
    libjpeg-dev \
    zlib1g-dev \
    libpng-dev \
    # docker client
    docker.io \
    # AWS CLI
    awscli

# docker compose
RUN curl -L "https://github.com/docker/compose/releases/download/${DockerComposeVersion}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose
RUN docker-compose --version
