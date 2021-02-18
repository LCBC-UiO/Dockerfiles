# Use Ubuntu 20.04
FROM ubuntu:20.04

# Disable interactive dialog when building image
ARG DEBIAN_FRONTEND=noninteractive

# Prepare environment
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        python-is-python3 \
        python3-pip \
        wget \
        git-annex \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install ukbfetch from ukbiobank
RUN wget -O /bin/ukbfetch -nd biobank.ctsu.ox.ac.uk/crystal/util/ukbfetch \
    && chmod 755 /bin/ukbfetch

# Install datalad-ukbiobank
RUN pip3 install datalad_ukbiobank
