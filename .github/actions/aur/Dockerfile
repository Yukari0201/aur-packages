FROM docker.io/library/archlinux:base-devel

RUN pacman -Syu --needed --noconfirm pacman-contrib namcap git sudo && \
  useradd -m builder && \
  echo 'builder ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

# Add archlinuxcn repository (prebuilt packages) and import its PGP keys
# Official flow from https://github.com/archlinuxcn/repo#usage, plus pacman-key --init
# (normally done by pacstrap; the docker base image has no secret key, which breaks
# archlinuxcn-keyring's local key signing on install)
RUN printf '[archlinuxcn]\nServer = https://repo.archlinuxcn.org/$arch\n' >> /etc/pacman.conf && \
  pacman-key --init && \
  pacman -Syy --noconfirm && \
  pacman -S --noconfirm --needed archlinuxcn-keyring

WORKDIR /home/builder
USER builder

# Install paru (prebuilt from archlinuxcn, no need to compile from AUR)
RUN sudo pacman -S --noconfirm --needed paru

COPY entrypoint.sh /entrypoint.sh

ENTRYPOINT [ "/entrypoint.sh" ]
