FROM mcr.microsoft.com/devcontainers/typescript-node:1-20-bookworm

# Accept build argument for including advanced tools
ARG INCLUDE_ADVANCED_TOOLS=true

# Install Python feature
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
  && apt-get -y install --no-install-recommends build-essential python3 python3-pip python3-venv

# Install Bun
RUN curl -fsSL https://bun.sh/install | bash

# Install Foundry
RUN curl -L https://foundry.paradigm.xyz | bash \
  && /bin/bash -c "source ~/.bashrc && ~/.foundry/bin/foundryup"

# Install slither
RUN pip install slither-analyzer --break-system-packages

# Install advanced tools (ityfuzz, z3, halmos) if INCLUDE_ADVANCED_TOOLS is true
RUN if [ "$INCLUDE_ADVANCED_TOOLS" = "true" ]; then \
  # Install ityfuzz
  curl -L https://ity.fuzz.land/ | bash \
  && /bin/bash -c "source ~/.bashrc && ~/.ityfuzz/bin/ityfuzzup" \
  # Install z3 and libssl-dev
  && apt-get update && export DEBIAN_FRONTEND=noninteractive \
  && apt-get -y install z3 libssl-dev \
  # Install halmos
  && pip install halmos --break-system-packages; \
  fi

# Set PATH to include installed tools
ENV PATH="/root/.foundry/bin:/root/.bun/bin:${PATH}"

# Add ityfuzz to PATH only if advanced tools are included
RUN if [ "$INCLUDE_ADVANCED_TOOLS" = "true" ]; then \
  echo 'export PATH="/root/.ityfuzz/bin:${PATH}"' >> /etc/bash.bashrc; \
  fi