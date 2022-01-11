FROM jenkins/jenkins:lts-alpine@sha256:5ac4c620e28b0b62ba6ca715ddbca3f661fafd522a3983292b1f784ca1d2fa56

ARG DOCKER_GID=974

USER root
RUN addgroup -g ${DOCKER_GID} docker && addgroup jenkins docker
RUN apk add patch

USER jenkins
RUN git config --global init.defaultBranch master
RUN jenkins-plugin-cli --plugins \
  build-monitor-plugin:1.13+build.202201031653 build-name-setter:2.2.0 \
  docker-plugin:1.2.6 docker-workflow:1.26 http_request:1.12 \
  pipeline-utility-steps:2.11.0 rebuild:1.32 view-job-filters:2.3 \
  ws-cleanup:0.40
