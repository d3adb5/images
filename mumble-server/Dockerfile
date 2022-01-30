FROM alpine AS download

ARG MURMUR_VERSION=1.3.4

ARG SERVER_BANDWIDTH=256000
ARG SERVER_PORT=64738

WORKDIR /murmur

ADD https://dl.mumble.info/stable/murmur-static_x86-${MURMUR_VERSION}.tar.bz2 murmur-static.tar.bz2

RUN tar xf murmur-static.tar.bz2 && mv murmur-static_x86-${MURMUR_VERSION} murmur-static
RUN sed -i "s/^bandwidth=.*/bandwidth=${SERVER_BANDWIDTH}/" murmur-static/murmur.ini && \
    sed -i "s/^port=.*/port=${SERVER_PORT}/" murmur-static/murmur.ini



FROM scratch

LABEL org.opencontainers.image.author="d3adb5" \
      org.label-schema.description="The Mumble server, built statically."

COPY --from=download /etc/passwd /etc/group /etc/
USER nobody

COPY --from=download --chown=nobody:nobody /murmur/murmur-static/murmur.x86 /murmur/murmur
COPY --from=download --chown=nobody:nobody /murmur/murmur-static/murmur.ini /murmur/murmur.ini

WORKDIR /murmur

EXPOSE 64738/tcp
EXPOSE 64738/udp

ENTRYPOINT ["/murmur/murmur", "-fg"]
