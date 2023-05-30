VERSION 0.7

FROM ubuntu:23.10

build:
  DO ./shared+BUILD
#  DO ./infrastructure+BUILD
#  DO ./rust+BUILD
#  DO ./typescript+BUILD
  DO +BUILD
  DO ./shared+CONFIG
  ENTRYPOINT ["bash"]
  SAVE IMAGE --push ghcr.io/hendric-dev/devcontainer

BUILD:
  COMMAND
  DO ./shared+RUN_ANSIBLE_SCRIPT --tags=cloud-sql-proxy,ruby,vector
#  DO +ORACLE

ORACLE:
  COMMAND
  # libaio1
  ARG BASE_URL=https://download.oracle.com/otn_software/linux/instantclient/217000
  RUN curl $BASE_URL/instantclient-basic-linux.x64-21.7.0.0.0dbru.zip -o instantclient && \
      curl $BASE_URL/instantclient-sdk-linux.x64-21.7.0.0.0dbru.zip -o instantclient-sdk && \
      mkdir -p $HOME/lib && \
      unzip instantclient -d $HOME/lib/oracle && \
      unzip instantclient-sdk -d $HOME/lib/oracle && \
      rm instantclient*
  ENV LD_LIBRARY_PATH=$HOME/lib/oracle/instantclient_21_7
  ENV PATH=$HOME/lib/oracle/instantclient_21_7:$PATH
