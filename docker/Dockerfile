ARG BASEIMAGE
FROM $BASEIMAGE

ARG USER
ARG GNAME
ARG UID
ARG GID

# Add user with given UID, GID, USER, GNAME
# Add user to sudo, GNAME, render group

# Set non-interactive frontend for apt-get
ENV DEBIAN_FRONTEND=noninteractive

# Install necessary packages
RUN apt-get update && \
    apt-get install -y --no-install-recommends sudo wget git && \
    rm -rf /var/lib/apt/lists/*

# Remove user/group if there is one using my ${UID} already.
RUN if id -u $UID >/dev/null 2>&1; then userdel -r $(getent passwd $UID | cut -d: -f1); fi
RUN if getent group $GID >/dev/null 2>&1; then groupdel $(getent group $GID | cut -d: -f1); fi

# Create group and user
RUN groupadd -g $GID $GNAME
RUN useradd -u $UID -g $GID -m -s /bin/bash $USER

# Add user to sudo, given group, and render group
RUN usermod -aG sudo $USER && \
    usermod -aG $GNAME $USER

# Set environment variables to allow sudo without password (optional)
RUN echo "$USER ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# For intel iGPU
ARG RENDERGID=-1
RUN if [ "$RENDERGID" -ne -1 ]; then \
        groupadd -g $RENDERGID host_render && \
        usermod -aG host_render $USER; \
    fi

# Bash configurations
RUN echo 'PATH=/home/$USER/.local/bin:$PATH' >> /home/$USER/.bashrc && \
    echo 'shopt -s histappend' >> /home/$USER/.bashrc && \
    echo 'shopt -s cmdhist' >> /home/$USER/.bashrc && \
    echo 'HISTCONTROL=ignoreboth' >> /home/$USER/.bashrc && \
    echo 'HISTSIZE=100000' >> /home/$USER/.bashrc && \
    echo 'HISTFILESIZE=200000' >> /home/$USER/.bashrc

# Add inputrc configurations for history search
RUN echo '"\\e[A": history-search-backward' >> /home/$USER/.inputrc && \
    echo '"\\e[B": history-search-forward' >> /home/$USER/.inputrc
  
# Install neovim and vifm
WORKDIR /home/$USER
RUN wget --no-check-certificate https://github.com/neovim/neovim/releases/download/v0.10.0/nvim-linux64.tar.gz && \
    tar -xzvf nvim-linux64.tar.gz && \
    rm nvim-linux64.tar.gz
