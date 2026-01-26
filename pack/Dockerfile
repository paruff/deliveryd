FROM buildpacksio/pack:latest

USER root

# Install additional tools for build context
RUN apk add --no-cache \
    bash \
    git \
    curl \
    jq

USER pack

WORKDIR /workspace

# Set default builder
ENV CNB_BUILDER=paketobuildpacks/builder:base

ENTRYPOINT ["pack"]
CMD ["--help"]
