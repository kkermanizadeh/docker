# Dockerfile to generate a container for Hyperledger Composer
# Based on ubuntu 16.04.4 LTS
FROM ubuntu:16.04
LABEL author="Kambiz Kermanizadeh"
LABEL authorEmail="kambizkermani@yahoo.com"
LABEL version="1.0"
LABEL description="Dockerfile to generate a container for Hyperledger Composer"

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

#Install different packages
RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    apt-get install -y build-essential \
                       libssl-dev \
                       apt-utils \
                       git \
                       zip \
                       curl \
                       nano \
                       gawk \
                       sudo \
                       libreadline6-dev \
                       libyaml-dev \
                       autoconf \
                       libgmp-dev \
                       libgdbm-dev \
                       libncurses5-dev \
                       automake \
                       libtool \
                       bison \
                       pkg-config \
                       libffi-dev \
                       automake \
                       nasm  \
                       libpng-dev\
                       libwebp-dev \
                       python \
                       python-dev \
                       python-pip \
                       python-virtualenv \
                       && \
     rm -rf /var/lib/apt/lists/*

ENV NVM_DIR /usr/local/.nvm
ENV NODE_VERSION stable

# Install nvm
RUN git clone https://github.com/creationix/nvm.git $NVM_DIR && \
    cd $NVM_DIR && \
    git checkout `git describe --abbrev=0 --tags`

# Install default version of Node.js
RUN source $NVM_DIR/nvm.sh && \
    nvm install $NODE_VERSION && \
    nvm install lts/boron && \
    nvm install lts/carbon && \
    nvm alias default $NODE_VERSION && \
    nvm use default

# Add nvm.sh to .bashrc for startup...
RUN echo "source ${NVM_DIR}/nvm.sh" > $HOME/.bashrc && \
    source $HOME/.bashrc

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules

# Set the path.
ENV PATH      $NVM_DIR/v$NODE_VERSION/bin:$PATH

#Create a defule user: composer
RUN useradd -p $(openssl passwd -1 'composer') -m -d /home/composer \
            -s /bin/bash -g root 'composer'

RUN groupadd -g 500 node

RUN usermod -a -G sudo,node composer
RUN echo "source /usr/local/.nvm/nvm.sh" > /home/composer/.bashrc && \
    source /home/composer/.bashrc

#Login as composer
RUN su - composer

#RUN npm install -g composer-cli
#RUN mkdir ~/HyperledgerComposer
#RUN export COMPOSER_ROOT=~/HyperledgerComposer

