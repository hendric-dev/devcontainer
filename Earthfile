VERSION 0.7

FROM ubuntu:23.04

build:
  DO ./shared+BUILD
  DO ./infrastructure+BUILD
  DO ./rust+BUILD
  DO ./typescript+BUILD
  DO +BUILD
  DO ./shared+CONFIG
  ENTRYPOINT ["bash", "-l"]
  SAVE IMAGE --push ghcr.io/hendric-dev/devcontainer

BUILD:
  COMMAND
  DO ./shared+RUN_ANSIBLE_SCRIPT --tags=cloud-sql-proxy,oracle-instant-client,ruby,vector
