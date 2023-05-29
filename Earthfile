VERSION 0.7

FROM ubuntu:23.10

build:
  DO ./shared+BUILD
  DO ./infrastructure+BUILD
  DO ./rust+BUILD
  DO ./typescript+BUILD
  DO +BUILD
  DO ./shared+CONFIG
  ENTRYPOINT ["bash"]
  SAVE IMAGE --push ghcr.io/hendric-dev/devcontainer

BUILD:
  COMMAND
  DO ./shared+RUN_ANSIBLE_SCRIPT --tags=cloud-sql-proxy,ruby,vector
  DO +SYSTEM_LIBRARIES
  DO +BENTHOS
  DO +KRAKEND --VERSION=2.2.1
  DO +ORACLE
  DO +PLAYWRIGHT

BENTHOS:
  COMMAND
  RUN curl -Lsf https://sh.benthos.dev | bash

KRAKEND:
  COMMAND
  ARG VERSION
  RUN curl https://repo.krakend.io/bin/krakend_${VERSION}_amd64_generic-linux.tar.gz -o krakend.tar.gz && \
      mkdir krakend && \
      tar -xvzf krakend.tar.gz -C krakend && \
      sudo mv krakend/usr/bin/krakend /usr/bin/krakend && \
      sudo chmod a+x /usr/bin/krakend && \
      rm -r krakend*

ORACLE:
  COMMAND
  ARG BASE_URL=https://download.oracle.com/otn_software/linux/instantclient/217000
  RUN curl $BASE_URL/instantclient-basic-linux.x64-21.7.0.0.0dbru.zip -o instantclient && \
      curl $BASE_URL/instantclient-sdk-linux.x64-21.7.0.0.0dbru.zip -o instantclient-sdk && \
      mkdir -p $HOME/lib && \
      unzip instantclient -d $HOME/lib/oracle && \
      unzip instantclient-sdk -d $HOME/lib/oracle && \
      rm instantclient*
  ENV LD_LIBRARY_PATH=$HOME/lib/oracle/instantclient_21_7
  ENV PATH=$HOME/lib/oracle/instantclient_21_7:$PATH

PLAYWRIGHT:
  COMMAND
  RUN npm install --location=global playwright

SYSTEM_LIBRARIES:
  COMMAND
  RUN sudo apt-get update && \
      sudo apt-get install -y \
        build-essential bc gnupg libaio1 libasound2 libatk1.0-0 libatk-bridge2.0-0 libatspi2.0-0 libcairo2 libcups2 \
        libdbus-1-3 libdbus-glib-1-2 libdrm2 libgbm1 libnss3 libnspr4 libpango-1.0-0 libssl-dev libx11-xcb1 \
        libxcomposite1 libxdamage1 libxfixes3 libxkbcommon0 libxrandr2 lsb-release pkg-config unzip \
        zlib1g-dev && \
      sudo rm -rf /var/lib/apt/lists/*
