FROM ubuntu:focal

# needed to install tzdata
ARG DEBIAN_FRONTEND=noninteractive
ARG MUMBLE_REPO=https://github.com/mumble-voip/mumble

RUN apt-get update && apt-get install --no-install-recommends -y \
	ca-certificates cmake git build-essential pkg-config curl \
	zip unzip tar \
	libx11-dev \
    mesa-common-dev \
    libxi-dev \
    libxext-dev \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists/*

RUN git clone --single-branch -b 1.4.x ${MUMBLE_REPO} /root/mumble
WORKDIR /root/mumble
RUN /root/mumble/scripts/vcpkg/get_mumble_dependencies.sh
WORKDIR /root/mumble/build

RUN git submodule update --init --recursive
RUN cmake -Dclient=OFF -Dstatic=ON -Dice=OFF \
	-DVCPKG_TARGET_TRIPLET=x64-linux \
	-DCMAKE_TOOLCHAIN_FILE=/root/vcpkg/scripts/buildsystems/vcpkg.cmake \
	-DCMAKE_BUILD_TYPE=Release .. || \
    ( cat \
    /root/mumble/build/CMakeFiles/CMakeOutput.log \
    /root/mumble/build/CMakeFiles/CMakeError.log \
    && false \
    )
RUN make -j $(nproc)

# Clean distribution stage
FROM ubuntu:focal

ARG DEBIAN_FRONTEND=noninteractive

RUN adduser murmur

COPY --from=0 /root/mumble/build/mumble-server /usr/bin/mumble-server
COPY --from=0 /root/mumble/scripts/murmur.ini /etc/murmur/murmur.ini

RUN mkdir /var/lib/murmur && \
	chown --verbose murmur:murmur /var/lib/murmur && \
	sed -i 's/^database=$/database=\/var\/lib\/murmur\/murmur.sqlite/' /etc/murmur/murmur.ini

EXPOSE 64738/tcp 64738/udp 50051
USER murmur

CMD /usr/bin/mumble-server -v -fg -ini /etc/murmur/murmur.ini
