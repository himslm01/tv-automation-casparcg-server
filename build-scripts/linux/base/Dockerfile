FROM buildpack-deps:trusty

ARG maintainer=mark.himsley@bbc.co.uk
LABEL maintainer=${maintainer}

ENV http_proxy=${http_proxy} \
    https_proxy=${https_proxy}

# The following best practices page says do not use 'apt-get -y upgrade'
# It also says to execute 'rm -rf /var/lib/apt/lists/*' at the end of the install
# https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/#apt-get
RUN apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get -yq --no-install-recommends install \
	cmake \
	time \
	git \
	lsb-release \
	libglew-dev \
	libfreeimage-dev \
	libtbb-dev \
	libsndfile1-dev \
	libopenal-dev \
	libxcursor-dev \
	libxinerama-dev \
	libxi-dev \
	libnih-dbus-dev \
	libsfml-dev \
	libxrandr-dev \
	libfdk-aac-dev \
	libjpeg-dev \
	libsndfile1-dev \
	libudev-dev \
	libglu1-mesa-dev \
	libharfbuzz0b \
	libpangoft2-1.0-0 \
	libcairo2 \
	libv4l-0 \
	libraw1394-11 \
	libavc1394-0 \
	libiec61883-0 \
	libxtst6 \
	libnss3 \
	libnspr4 \
	libgconf-2-4 \
	libasound2 \
	libfreeimage-dev \
	libglew-dev \
	libopenal-dev \
	libopus-dev \
	libgsm1-dev \
	libmodplug-dev \
	libass-dev \
	libbluray-dev \
	libfribidi-dev \
	libgmp-dev \
	libmp3lame-dev \
	libopencore-amrnb-dev \
	libopencore-amrwb-dev \
	librtmp-dev \
	libtheora-dev \
	libx264-dev \
	libxvidcore-dev \
	libxss1 \
	libgtk2.0-0 \
	libssl1.0.0 \
	libexpat1-dev \
	libssl-dev \
	zlib1g-dev \
 && rm -rf /var/lib/apt/lists/*

# ffmpeg requires libvpx decoder version >=1.4.0
#	libvpx-dev \

# not available in trusty
#	libglfw3-dev \
#	libx265-dev \
# Conflicts with gnutls-dev
# 	libgnutls28-dev \

WORKDIR /opt/casparcg

