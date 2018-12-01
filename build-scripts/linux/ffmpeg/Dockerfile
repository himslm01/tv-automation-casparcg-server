ARG casparcg_from="casparcg_base"
FROM ${casparcg_from} as BUILD

ARG maintainer=mark.himsley@bbc.co.uk
LABEL maintainer=${maintainer}

ENV http_proxy=${http_proxy} \
    https_proxy=${https_proxy}

ARG FFMPEG_COMMIT="acdea9e7c56b74b05c56b4733acc855b959ba073"
ARG BUILD_PARALLEL_THREADS=4
ENV FFMPEG_COMMIT="${FFMPEG_COMMIT}" \
    BUILD_PARALLEL_THREADS="${BUILD_PARALLEL_THREADS}"

# The following best practices page says do not use 'apt-get -y upgrade'
# It also says to execute 'rm -rf /var/lib/apt/lists/*' at the end of the install
# https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#apt-get
RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
	autoconf \
	automake \
	cmake \
	curl \
	bzip2 \
	libexpat1-dev \
	g++ \
	gcc \
	git \
	gperf \
	libtool \
	make \
	nasm \
	perl \
	pkg-config \
	python \
	libssl-dev \
	yasm \
	zlib1g-dev \
 && rm -rf /var/lib/apt/lists/*


RUN wget https://github.com/FFmpeg/FFmpeg/archive/${FFMPEG_COMMIT}.tar.gz \
 && tar zxf ${FFMPEG_COMMIT}.tar.gz \
 && rm ${FFMPEG_COMMIT}.tar.gz

WORKDIR FFmpeg-${FFMPEG_COMMIT}

RUN ./configure \
	--enable-version3 \
	--enable-gpl \
	--enable-nonfree \
	--enable-small \
	--enable-libmp3lame \
	--enable-libx264 \
	--enable-libtheora \
	--enable-libvorbis \
	--enable-libopus \
	--enable-libfdk-aac \
	--enable-libass \
	--enable-libwebp \
	--enable-librtmp \
	--enable-postproc \
	--enable-avresample \
	--enable-libfreetype \
	--enable-openssl \
	--disable-debug \
	--disable-ffplay \
    --disable-dxva2 \
	--prefix=/opt/ffmpeg \
 && make -j${BUILD_PARALLEL_THREADS} \
 && make install

# libvpx decoder version must be >=1.4.0
#	--enable-libvpx \

# not available in trusty
#	--enable-libx265 \

FROM scratch
	COPY --from=BUILD /opt/ffmpeg /opt/ffmpeg
