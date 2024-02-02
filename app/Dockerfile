FROM debian:11

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
        
RUN wget -qO- https://deb.nodesource.com/setup_10.x | bash

#RUN apt-get install -y nodejs

RUN rm -rf /var/lib/apt/lists/*

# Install Yarn globally
RUN npm install -g yarn


# install LL app
ENV LL_TAG=v7.1.0
RUN git clone https://github.com/metedemirtas26/learninglocker.git /opt/learninglocker
RUN cd /opt/learninglocker
WORKDIR /opt/learninglocker
RUN git checkout $LL_TAG || true
RUN yarn install

#RUN yarn install --ignore-engines
RUN yarn --ignore-engines build-all 

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

