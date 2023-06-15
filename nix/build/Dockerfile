FROM docker.io/heywoodlh/trizen:latest

RUN sudo useradd -m -s /bin/bash heywoodlh \
    && echo 'heywoodlh ALL=(ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/heywoodlh

USER heywoodlh
WORKDIR /home/heywoodlh

RUN sudo userdel trizen \
    && sudo rm -rf /home/trizen \
    && sudo rm -rf /etc/sudoers.d/trizen

# Core dependencies
RUN sudo pacman -Sy --noconfirm inetutils openssh \
    && sudo pacman -Sc --noconfirm

# 1password-cli
RUN gpg --receive-keys 3FEF9748469ADBE15DA7CA80AC2D62742012EA22 \
    && trizen -S --noconfirm 1password-cli

ENV NP_GIT=/usr/bin/git
ENV USER='heywoodlh'

RUN sh <(curl -L https://nixos.org/nix/install) --no-daemon \
    && sudo install -d -m755 -o $(id -u) -g $(id -g) /nix \
    && source $HOME/.nix-profile/etc/profile.d/nix.sh \
    && git clone https://github.com/heywoodlh/nixos-configs ~/opt/nixos-configs \
    && /home/heywoodlh/.nix-profile/bin/nix --extra-experimental-features "nix-command flakes" --keep-going run ~/opt/nixos-configs#homeConfigurations.heywoodlh-server.activationPackage --impure

RUN sudo chsh -s /home/heywoodlh/.nix-profile/bin/zsh heywoodlh

ENTRYPOINT ["/home/heywoodlh/.nix-profile/bin/zsh"]
