VERSION 0.6

FROM ubuntu:22.04

build:
  DO ./shared+BUILD
  DO ./flutter+BUILD
  DO ./infrastructure+BUILD
  DO ./rust+BUILD
  DO ./typescript+BUILD
  DO +BUILD
  DO ./shared+CONFIG
  ENTRYPOINT ["bash"]
  SAVE IMAGE --push ghcr.io/hendric-dev/devcontainer

BUILD:
  COMMAND
  DO +SYSTEM_LIBRARIES
  DO +BENTHOS
  DO +KRAKEND --VERSION=2.2.1
  DO +ORACLE
  DO +PLAYWRIGHT
  DO +RUBY --VERSION=3.2.1
  DO +GEMS
  DO +VECTOR

BENTHOS:
  COMMAND
  RUN curl -Lsf https://sh.benthos.dev | bash

GEMS:
  COMMAND
  RUN eval "$(rbenv init - bash)" && \
      gem install bundler solargraph

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

RUBY:
  COMMAND
  ARG VERSION
  ENV PATH="$HOME/.rbenv/bin:$PATH"
  RUN git clone https://github.com/rbenv/rbenv.git $HOME/.rbenv && \
      mkdir -p $HOME/.rbenv/plugins && \
      git clone https://github.com/rbenv/ruby-build.git $HOME/.rbenv/plugins/ruby-build && \
      echo 'eval "$(rbenv init - bash)"' >> $HOME/.bashrc
  RUN rbenv install $VERSION && \
      rbenv global $VERSION

SYSTEM_LIBRARIES:
  COMMAND
  RUN sudo apt-get update && \
      sudo apt-get install -y \
        build-essential bc gnupg libaio1 libasound2 libatk1.0-0 libatk-bridge2.0-0 libatspi2.0-0 libcairo2 libcups2 \
        libdbus-1-3 libdbus-glib-1-2 libdrm2 libgbm1 libnss3 libnspr4 libpango-1.0-0 libssl-dev libx11-xcb1 \
        libxcomposite1 libxdamage1 libxfixes3 libxkbcommon0 libxrandr2 libyaml-dev lsb-release pkg-config unzip \
        zlib1g-dev && \
      sudo rm -rf /var/lib/apt/lists/*

VECTOR:
  COMMAND
  RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.vector.dev | bash -s -- -y
  RUN sudo ln -s $HOME/.vector/bin/vector /usr/bin/vector
