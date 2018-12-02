ARG casparcg_from="casparcg_base"
FROM ${casparcg_from}

# --from should be a build ARG, but cannot be due to:
# https://github.com/moby/moby/issues/35018
COPY --from=casparcg_ffmpeg:2.1.x-linux_build_14.04 /opt/ffmpeg /opt/ffmpeg
COPY ./ /opt/casparcg/

ARG BUILD_ARCHIVE_NAME="CasparCG Server"
ARG BUILD_PARALLEL_THREADS=4
ARG GIT_HASH
ENV BUILD_ARCHIVE_NAME="${BUILD_ARCHIVE_NAME}" \
    BUILD_PARALLEL_THREADS="${BUILD_PARALLEL_THREADS}" \
    GIT_HASH="${GIT_HASH}"

RUN cd build-scripts && ./build-linux.sh