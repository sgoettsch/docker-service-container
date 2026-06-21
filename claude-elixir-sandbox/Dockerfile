FROM docker/sandbox-templates:claude-code@sha256:72570e76fbb20c9bd8b945efea169fe0f2e0696e80d489125e1206d6d409ee91

USER root

# Install system dependencies for Elixir/Erlang compilation
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    wget \
    git \
    build-essential \
    autoconf \
    libncurses5-dev \
    libssl-dev \
    libssh-dev \
    inotify-tools \
    postgresql-client \
    nodejs \
    npm \
    && rm -rf /var/lib/apt/lists/*

# Install asdf version manager system-wide
RUN git clone https://github.com/asdf-vm/asdf.git /opt/asdf --branch v0.14.0

ENV ASDF_DIR=/opt/asdf

RUN echo 'export ASDF_DIR=/opt/asdf' >> /etc/bash.bashrc && \
    echo '. /opt/asdf/asdf.sh' >> /etc/bash.bashrc && \
    echo 'export ASDF_DIR=/opt/asdf' >> /etc/profile && \
    echo '. /opt/asdf/asdf.sh' >> /etc/profile

USER agent

# Setup asdf for agent user
RUN echo 'export ASDF_DIR=/opt/asdf' >> /home/agent/.bashrc && \
    echo '. /opt/asdf/asdf.sh' >> /home/agent/.bashrc && \
    echo 'export ASDF_DIR=/opt/asdf' >> /home/agent/.profile && \
    echo '. /opt/asdf/asdf.sh' >> /home/agent/.profile

# Skip optional GUI/Java components — not needed in a headless sandbox
ENV KERL_CONFIGURE_OPTIONS="--without-javac --without-odbc --without-wx --without-debugger --without-observer --without-et"

# Install Erlang and Elixir via asdf
RUN . /opt/asdf/asdf.sh && \
    asdf plugin add erlang https://github.com/asdf-vm/asdf-erlang.git && \
    asdf plugin add elixir https://github.com/asdf-vm/asdf-elixir.git && \
    asdf install erlang 27.1 && \
    asdf global erlang 27.1 && \
    asdf install elixir 1.17.3-otp-27 && \
    asdf global elixir 1.17.3-otp-27

# Install Hex and Rebar
RUN . /opt/asdf/asdf.sh && \
    mix local.hex --force && \
    mix local.rebar --force

WORKDIR /workspace
