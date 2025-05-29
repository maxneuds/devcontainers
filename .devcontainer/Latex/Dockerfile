FROM debian:trixie-slim
ENV DEBIAN_FRONTEND=noninteractive

# Enable arguments and set username
ENV USERNAME=dev
ARG UID
ARG GID
ARG DOCKER_GID

# Update repo
RUN apt-get clean && apt-get update --fix-missing

# Make sure locals are set to en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
RUN apt-get install -y locales
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen

# <- LaTeX ->
RUN apt-get update && apt-get install -y \
    texlive-full latexmk chktex make
# <- LaTeX ->

# <- DEVCONTAINER ->
# Setup container user, groups and workspace
RUN groupadd -g ${GID} ${USERNAME} && \
    useradd -m -u ${UID} -g ${GID} ${USERNAME} && \
    mkdir /workspace && chown ${USERNAME}:${USERNAME} /workspace
# Install zsh and other useful packages
RUN apt-get install -y \
    git wget curl unzip rsync zsh htop vim
# Setup the environment for the container user
RUN chsh -s /bin/zsh ${USERNAME}
# Switch to container user and set home directory
USER ${USERNAME}
# Install oh-my-zsh
RUN sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)" "" --unattended
# enable command completion and git support in zsh
RUN git clone https://github.com/zsh-users/zsh-completions ~/.oh-my-zsh/custom/plugins/zsh-completions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions
RUN sed -i 's/plugins=(git)/plugins=(git zsh-completions zsh-syntax-highlighting zsh-autosuggestions)/' ~/.zshrc
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="agnoster"/' ~/.zshrc
RUN echo 'prompt_context() {}' >> ~/.zshrc
# Command alias
RUN echo 'alias ll="ls -lah --color=auto"' >> ~/.zshrc
RUN echo 'alias cp="rsync -avh --progress"' >> ~/.zshrc
# Switch back to root user
USER root
# Apt cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
# <- DEVCONTAINER ->

# Switch user
USER ${USERNAME}
