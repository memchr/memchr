FROM archlinux

# Sync db
RUN pacman -Syu --noconfirm --noprogressbar

# Base packages
RUN pacman -S base base-devel git linux-tools \
	--needed --noconfirm --noprogressbar

# docker
# Additional packages
RUN pacman -S grc ripgrep fd bat gdu lf age unzip meson cmake neovim fzf zsh \
	gnome-keyring libxkbfile nss libnotify libxss lsof github-cli fish aichat \
	openssh docker \
	--needed --noconfirm --noprogressbar
RUN groupadd -rf docker

# docker
COPY init-docker.sh /usr/local/lib/init-docker.sh
RUN chmod +x /usr/local/lib/init-docker.sh
# Symlink nvim to vi
WORKDIR /bin
RUN ln -sf nvim vi

# locale
RUN echo "LANG=en_US.UTF-8" | tee /etc/locale.conf && \
	echo "en_US.UTF-8 UTF-8" | tee -a /etc/locale.gen && \
	locale-gen
# Add user
RUN useradd -mG wheel,docker codespace && \
	echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' | sudo EDITOR='tee -a' visudo && \
	chsh codespace -s /bin/fish

# Cleanup
RUN yes | pacman -Scc
ENTRYPOINT ["/usr/local/lib/init-docker.sh"]
