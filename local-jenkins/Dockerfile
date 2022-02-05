FROM jenkins/jenkins:lts-alpine@sha256:8a18b4ae96f80d364d085375fc6d40f25ee16a8c6e4ae3d4d7e95bf14db48059

ARG DOCKER_GID=974

USER root
RUN addgroup -g ${DOCKER_GID} docker && addgroup jenkins docker

USER jenkins
RUN jenkins-plugin-cli --plugins \
  build-monitor-plugin:1.13+build.202201031653 build-name-setter:2.2.0 \
  docker-plugin:1.2.6 docker-workflow:1.26 http_request:1.12 \
  pipeline-utility-steps:2.11.0 rebuild:1.32 remote-file:1.21 \
  view-job-filters:2.3 ws-cleanup:0.40
