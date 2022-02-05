FROM jenkins/jenkins:lts-alpine@sha256:8a18b4ae96f80d364d085375fc6d40f25ee16a8c6e4ae3d4d7e95bf14db48059

ARG DOCKER_GID=974

USER root
RUN addgroup -g ${DOCKER_GID} docker && addgroup jenkins docker

USER jenkins
RUN jenkins-plugin-cli --plugins \
  build-monitor-plugin build-name-setter docker-plugin docker-workflow \
  http_request pipeline-utility-steps rebuild remote-file view-job-filters \
  ws-cleanup
