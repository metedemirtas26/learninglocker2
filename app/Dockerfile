FROM debian:10

RUN apt-get update

RUN apt-get install -y \
        wget \
        procps \
        curl \
        git \
        python \
        build-essential \
        xvfb \
        apt-transport-https \
        unzip \
        gettext-base \
        socat \
        npm
        
RUN wget -qO- https://deb.nodesource.com/setup_8.x | bash

RUN apt-get install -y nodejs

RUN rm -rf /var/lib/apt/lists/*

# Install Yarn globally
RUN npm install -g yarn@latest


RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash \
    && export NVM_DIR="$HOME/.nvm" \
    && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
    && [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" \
    && nvm install 8.17.0 \
    && nvm use 8.17.0

# install LL app
ENV LL_TAG=v2.8.2
RUN git clone https://github.com/metedemirtas26/learninglocker /opt/learninglocker

RUN cd /opt/learninglocker

RUN git checkout v2.8.2 || true

# Set working directory
WORKDIR /opt/learninglocker

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash \
    && export NVM_DIR="$HOME/.nvm" \
    && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
    && [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" \
    && nvm install 8.17.0 \
    && nvm use 8.17.0

RUN yarn install

RUN yarn build-all

# make a copy of the origin storage directory that will be used in entrypoint-common.sh
# to fill up a volume mounted here, if it's empty
RUN cp -r storage storage.template

# service api exposes port 8080
# service ui exposes port 3000
# see env.template
EXPOSE 3000 8080

COPY env.template .env.template
COPY entrypoint-common.sh entrypoint-common.sh
COPY entrypoint-ui.sh entrypoint-ui.sh

ENTRYPOINT ["./entrypoint-common.sh"]

