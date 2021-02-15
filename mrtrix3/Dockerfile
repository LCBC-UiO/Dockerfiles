# Use Ubuntu 20.04 LTS
FROM ubuntu:20.04

# Disable interactive dialog when building image
ARG DEBIAN_FRONTEND=noninteractive

# Prepare environment
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git \
        g++ \
        python-is-python3 \
        libeigen3-dev \
        zlib1g-dev \
        libqt5opengl5-dev \
        libqt5svg5-dev \
        libgl1-mesa-dev \
        libfftw3-dev \
        libtiff5-dev \
        libpng-dev \
        unzip \
        ca-certificates \
        curl && \
    apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install mrtrix3 from source
ARG MRTRIX_SHA=3.0.2
ENV PATH="/opt/mrtrix3-${MRTRIX_SHA}/bin:$PATH"
RUN cd /opt \
    && curl -sSLO https://github.com/MRtrix3/mrtrix3/archive/${MRTRIX_SHA}.zip \
    && unzip ${MRTRIX_SHA}.zip \
    && rm ${MRTRIX_SHA}.zip \
    && cd /opt/mrtrix3-${MRTRIX_SHA} \
    && ./configure -nogui \
    && echo "Compiling MRtrix3 ..." \
    && ./build