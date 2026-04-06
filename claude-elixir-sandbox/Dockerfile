FROM docker/sandbox-templates:claude-code

USER root

# Install system dependencies for Elixir/Erlang
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    wget \
    git \
    build-essential \
    autoconf \
    libncurses5-dev \
    libssl-dev \
    libwxgtk3.2-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    libpng-dev \
    libssh-dev \
    unixodbc-dev \
    xsltproc \
    fop \
    libxml2-utils \
    inotify-tools \
    postgresql-client \
    nodejs \
    npm \
    && rm -rf /var/lib/apt/lists/*

# Install asdf version manager as root first
RUN git clone https://github.com/asdf-vm/asdf.git /opt/asdf --branch v0.14.0

# Make asdf available system-wide
RUN echo '. /opt/asdf/asdf.sh' >> /etc/bash.bashrc && \
    echo '. /opt/asdf/asdf.sh' >> /etc/profile

USER agent

# Setup asdf for agent user
RUN echo '. /opt/asdf/asdf.sh' >> /home/agent/.bashrc && \
    echo '. /opt/asdf/asdf.sh' >> /home/agent/.profile

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
