FROM archlinux

# Sync db
RUN pacman -Syu --noconfirm --noprogressbar

# Base packages
RUN pacman -S base base-devel git linux-tools \
	--needed --noconfirm --noprogressbar

# Additional packages
RUN pacman -S grc ripgrep fd bat gdu lf age meson cmake neovim fzf zsh \
	gnome-keyring chezmoi libxkbfile nss libnotify libxss lsof github-cli \
	--needed --noconfirm --noprogressbar

# Additional configurations
# Symlink nvim to vi
WORKDIR /bin
RUN ln -sf nvim vi
# Change ZDOTDIR to $XDG_CONFIG_HOME/zsh
RUN echo 'export ZDOTDIR="${XDG_CONFIG_HOME:-$HOME/.config}/zsh"' | tee -a /etc/zsh/zshenv

# locale
RUN echo "LANG=en_US.UTF-8" | tee /etc/locale.conf && \
	echo "en_US.UTF-8 UTF-8" | tee -a /etc/locale.gen && \
	locale-gen

# Add user
RUN useradd -mG wheel codespace && \
	echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' | sudo EDITOR='tee -a' visudo && \
	chsh codespace -s /bin/zsh

# AUR helper
WORKDIR /home/codespace
USER codespace
RUN mkdir paru-bin && cd paru-bin && \
	curl 'https://aur.archlinux.org/cgit/aur.git/plain/PKGBUILD?h=paru-bin' -o PKGBUILD && \
	makepkg -csi --noconfirm --noprogressbar

# Dotfiles
RUN chezmoi init --apply --promptString name="codespace",email="none@none.none" \
	--promptBool trusted=false,install=false memchr
RUN zsh -i -c 'exit'

# Cleanup
RUN rm -rf /home/codespace/paru-bin
USER root
RUN yes | pacman -Scc
