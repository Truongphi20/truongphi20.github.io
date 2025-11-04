FROM ruby:3.1

# Install essentials
RUN apt-get update && apt-get install -y \
    build-essential \
    zlib1g-dev \
    nodejs \
    && rm -rf /var/lib/apt/lists/*

# Install Jekyll + Bundler
RUN gem install bundler jekyll


# Set up workspace
RUN mkdir -p /workspace

WORKDIR /workspace

# Add colorful, git-aware PS1
RUN echo 'parse_git_branch() {' >> /home/vscode/.bashrc \
 && echo '  branch=$(git branch --show-current 2>/dev/null)' >> /home/vscode/.bashrc \
 && echo ' if [ -n "$branch" ]; then echo " (\033[0;33m$branch\033[0m)"; fi' >> /home/vscode/.bashrc \
 && echo '}' >> /home/vscode/.bashrc \
 && echo 'export PS1="\[\033[0;32m\]\u@\h\[\033[0m\]->\[\033[0;34m\]\w\[\033[0m\]\$(parse_git_branch) \$ "' >> /home/vscode/.bashrc

# Change ownership so non-root can write to workspace
ARG USER_UID=1000
ARG USER_GID=1000
RUN groupmod -g $USER_GID vscode && usermod -u $USER_UID -g $USER_GID vscode
RUN chown -R vscode:vscode /workspace

# Switch to non-root user
USER vscode

# Make bash default shell
SHELL ["/bin/bash", "-c"]
CMD ["bash"]
