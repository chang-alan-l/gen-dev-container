FROM mambaorg/micromamba:jammy

# General dev tooling setup
# git, wget, and curl
USER root
RUN apt-get update && apt-get install -y --no-install-recommends \
    bzip2 \
    wget \
    gcc \
    g++ \
    gcc-11 \
    g++-11 \
    ca-certificates \
    curl \
    git \
    gh \
    make \
    tmux \
    zsh

# node (required by neovim)
RUN curl -fsSL https://deb.nodesource.com/setup_19.x | bash - && \
    apt-get install -y --no-install-recommends nodejs

# neovim
RUN wget https://github.com/neovim/neovim/releases/download/stable/nvim-linux64.deb -O nvim.deb && \
    apt-get install ./nvim.deb && \
    rm -rf /var/lib/apt /var/lib/dpkg /var/lib/cache /var/lib/log

# antigen + starship
RUN mkdir "/home/${MAMBA_USER}/external_tools/" && \
    curl -L git.io/antigen > "/home/${MAMBA_USER}/external_tools/antigen.zsh" && \
    curl -sS https://starship.rs/install.sh | sh -s -- --yes

# direnv
RUN curl -sfL https://direnv.net/install.sh | bash

# tmux + config
RUN git clone https://github.com/gpakosz/.tmux.git "/home/${MAMBA_USER}/.tmux" && \
    ln -s -f "/home/${MAMBA_USER}/.tmux/.tmux.conf" "/home/${MAMBA_USER}.tmux.conf"

# Micromamba Setup
RUN micromamba shell init -s zsh && \
    micromamba shell init -s bash
ARG MAMBA_DOCKERFILE_ACTIVATE=1

# neovim config
RUN mkdir -p "/home/${MAMBA_USER}/.config/nvim/" && \
    micromamba install -y -c conda-forge PyYAML && \
    export HOME="/home/${MAMBA_USER}" && \
    git clone https://github.com/rafi/vim-config.git "/home/${MAMBA_USER}/.config/nvim" && \
    cd "/home/${MAMBA_USER}/.config/nvim/" && \
    make

# Requires import of dotfiles and install script
RUN git clone https://github.com/chang-alan-l/dotfiles.git "/home/${MAMBA_USER}/dotfiles/" && \
    cd "/home/${MAMBA_USER}/dotfiles/" && \
    export HOME="/home/${MAMBA_USER}" && \
    ./install.sh

USER $MAMBA_USER
WORKDIR "/home/${MAMBA_USER}"
CMD ["/bin/zsh"]
# COPY --chown=$MAMBA_USER:$MAMBA_USER env/dev.yml /tmp/env.yaml
# RUN micromamba install -y -n base -f /tmp/env.yaml && \
#    micromamba clean --all --yes
# ARG MAMBA_DOCKERFILE_ACTIVATE=1  # (otherwise python will not be found)

