ARG VERSION_PARENT=v0.0.49

FROM gitlab-registry.cern.ch/swan/docker-images/jupyter/swan-cern:${VERSION_PARENT}

LABEL maintainer="swan-admins@cern.ch"
ARG NB_UID="1000"
ARG BUILD_TAG=daily
ENV VERSION_DOCKER_IMAGE=$BUILD_TAG
ENV ACCPY_PATH="/opt/acc-py"

RUN echo "Building swan-accpy image with tag ${VERSION_DOCKER_IMAGE} from parent tag ${VERSION_PARENT}."

USER root

# Install currently supported acc-py distributions
RUN ACCPY_DIST="2020.11 2021.12 2023.06" && \
    for version in ${ACCPY_DIST}; do \
        wget http://bewww.cern.ch/ap/acc-py/installers/acc-py-${version}-installer.sh && \
        chmod +x acc-py-${version}-installer.sh && \
        ./acc-py-${version}-installer.sh --installation-root ${ACCPY_PATH} && \
        rm -f acc-py-${version}-installer.sh; \
    done

# Install Java JDK 11
RUN dnf install -y \
    java-11-openjdk.x86_64 && \
    dnf clean all && \
    rm -rf /var/cache/dnf

# Install UV for faster package installation
RUN export XDG_BIN_HOME=/usr/local/bin && \
    curl -LsSf https://astral.sh/uv/install.sh | sh -s -- --no-modify-path && \
    chmod u+x $XDG_BIN_HOME/uv

USER ${NB_UID}
