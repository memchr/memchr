FROM archlinux

# Sync db
RUN pacman -Syu --noconfirm --noprogressbar

# Base packages
RUN pacman -S base base-devel git linux-tools \
	--needed --noconfirm --noprogressbar

# Additional packages
RUN pacman -S grc ripgrep fd bat gdu lf age meson cmake neovim zsh \
	gnome-keyring xdg-utils chezmoi \
	--needed --noconfirm --noprogressbar

RUN yes | pacman -Scc

# locale
RUN echo "LANG=en_US.UTF-8" | tee /etc/locale.conf && \
	echo "en_US.UTF-8 UTF-8" | tee -a /etc/locale.gen && \
	locale-gen

# Symlink nvim to vi
WORKDIR /bin
RUN ln -sf nvim vi

# Add user
RUN useradd -mG wheel codespace && \
	echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' | sudo EDITOR='tee -a' visudo && \
	chsh codespace -s /bin/zsh

# AUR helper
WORKDIR /tmp
USER codespace
RUN curl 'https://aur.archlinux.org/cgit/aur.git/plain/PKGBUILD?h=paru-bin' -o PKGBUILD && \
	cd /tmp/paru-bin && makepkg -csi --noconfirm --noprogressbar
