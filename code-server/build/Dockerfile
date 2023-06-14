FROM docker.io/ubuntu:22.04

RUN apt-get update \
    && apt-get install -y \
    curl \
    git \
    golang \
    sudo \
    vim \
    wget \
    openssh-client \
    gnupg \
    && rm -rf /var/lib/apt/lists/*

ARG USER=heywoodlh
RUN useradd --groups sudo -m --shell /bin/bash ${USER} \
    && echo "${USER} ALL=(ALL) NOPASSWD:ALL" | tee /etc/sudoers.d/${USER} \
	&& chmod 0440 /etc/sudoers.d/${USER}

# Install 1Password-cli
RUN curl -sS https://downloads.1password.com/linux/keys/1password.asc | \
    gpg --dearmor --output /usr/share/keyrings/1password-archive-keyring.gpg &&\
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/1password-archive-keyring.gpg] https://downloads.1password.com/linux/debian/amd64 stable main" | \
    tee /etc/apt/sources.list.d/1password.list &&\
    mkdir -p /etc/debsig/policies/AC2D62742012EA22/ &&\
    curl -sS https://downloads.1password.com/linux/debian/debsig/1password.pol | \
    tee /etc/debsig/policies/AC2D62742012EA22/1password.pol &&\
    mkdir -p /usr/share/debsig/keyrings/AC2D62742012EA22 &&\
    curl -sS https://downloads.1password.com/linux/keys/1password.asc | \
    gpg --dearmor --output /usr/share/debsig/keyrings/AC2D62742012EA22/debsig.gpg &&\
    apt-get update && apt-get install -y 1password-cli &&\
    rm -rf /var/lib/apt/lists/*

# Install coder
RUN curl -fsSL https://coder.com/install.sh | sh \
    && rm -rf /var/lib/apt/lists

USER ${USER}
WORKDIR /home/${USER}

RUN sudo curl -L 'https://github.com/DavHau/nix-portable/releases/download/v009/nix-portable' -o /usr/local/bin/nix \
    && sudo chmod +x /usr/local/bin/nix

ENV NP_GIT="/usr/bin/git"

# Install code-server && extensions
RUN curl -fsSL https://code-server.dev/install.sh | sh \
    && code-server \
      --install-extension 1password.op-vscode \
      --install-extension arcticicestudio.nord-visual-studio-code \
      --install-extension eamodio.gitlens \
      --install-extension github.copilot \
      --install-extension jnoortheen.nix-ide \
      --install-extension ms-python.python \
      --install-extension vscodevim.vim
