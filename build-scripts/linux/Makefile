SHELL := /bin/bash
DATE := $(shell date +'%Y%m%d%H%M')
GIT_HASH := $(shell git rev-parse --verify --short HEAD)
CASPARCG_VERSION := $(shell git branch | sed -n -e 's/^\* \(.*\)/\1/p')

define HELP
targets:
	build
		runs build-base, build-ffmpeg and build-casparcg below

	build-base
		makes an image containing all of the required source code and libraries

	build-ffmpeg
		builds the appropriate ffmpeg in a Docker container and tags it as an image

	build-casparcg
		builds CasparCG in a Docker container from the source code above
	
	extract-casparcg
		copy the built "CasparCG Server.tar.gz" file from the casparcg_build image

	clean-exited
		removes ALL containers which are not running

	clean-dangling
		removes ALL images which are not referenced
endef
export HELP
default:
	@echo "$${HELP}"

build: build-base build-ffmpeg build-casparcg

build-base:
	pushd base/ >/dev/null && docker build \
	--build-arg http_proxy="${http_proxy}" \
	--build-arg https_proxy="${https_proxy}" \
	-t "casparcg_base:${CASPARCG_VERSION}" \
	$${PWD}

build-ffmpeg:
	pushd ffmpeg/ >/dev/null && docker build \
	--build-arg casparcg_from="casparcg_base:${CASPARCG_VERSION}" \
	--build-arg http_proxy="${http_proxy}" \
	--build-arg https_proxy="${https_proxy}" \
	-t "casparcg_ffmpeg:${CASPARCG_VERSION}" \
	$${PWD}

build-casparcg:
	pushd ../../ >/dev/null && docker build \
	--build-arg casparcg_from="casparcg_base:${CASPARCG_VERSION}" \
	--build-arg casparcg_from_ffmpeg="casparcg_ffmpeg:${CASPARCG_VERSION}" \
	--build-arg GIT_HASH="${GIT_HASH}" \
	--build-arg BUILD_PARALLEL_THREADS=2 \
	-f build-scripts/linux/casparcg/Dockerfile \
	-t "casparcg_build:${CASPARCG_VERSION}" \
	$${PWD}

extract-casparcg:
	set -e; \
	cleanup() { docker rm -v $$tempContainer; }; \
	tempContainer=$$(docker create "casparcg_build:${CASPARCG_VERSION}" sh); \
	trap "cleanup" EXIT ; \
	docker cp $$tempContainer:"/opt/casparcg/build/CasparCG Server.tar.gz" .

clean: clean-exited clean-dangling

clean-exited:
	set -e ;\
	CONTAINERS=$$(docker ps -aq --filter "status=exited") ;\
	if [ "Z$$CONTAINERS" != "Z" ]; then \
		echo $$CONTAINERS | xargs docker rm -v ;\
	fi

clean-dangling:
	set -e ;\
	IMAGES=$$(docker images -q --filter "dangling=true") ;\
	if [ "Z$$IMAGES" != "Z" ]; then \
		echo $$IMAGES | xargs docker rmi ;\
	fi
