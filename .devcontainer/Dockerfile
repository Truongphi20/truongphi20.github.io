FROM ruby:3.1

# Install essentials
RUN apt-get update && apt-get install -y \
    build-essential \
    zlib1g-dev \
    nodejs \
    && rm -rf /var/lib/apt/lists/*

# Install Bundler only
RUN gem install bundler

# Install GitHub Pages dependencies
COPY Gemfile /src/Gemfile
WORKDIR /src
RUN bundle install

# Workspace directory
RUN mkdir -p /workspace
WORKDIR /workspace

# Create vscode user
ARG USER_UID=1000
ARG USER_GID=1000
RUN groupadd -g $USER_GID vscode && \
    useradd -m -s /bin/bash -u $USER_UID -g $USER_GID vscode && \
    chown -R vscode:vscode /workspace

# Switch to non-root user
USER vscode

# Add colorful git-aware prompt
RUN echo 'parse_git_branch() {' >> ~/.bashrc \
 && echo '  branch=$(git branch --show-current 2>/dev/null)' >> ~/.bashrc \
 && echo '  if [ -n "$branch" ]; then echo " (\033[0;33m$branch\033[0m)"; fi' >> ~/.bashrc \
 && echo '}' >> ~/.bashrc \
 && echo 'export PS1="\[\033[0;32m\]\u@\h\[\033[0m\]->\[\033[0;34m\]\w\[\033[0m\]\$(parse_git_branch) \$ "' >> ~/.bashrc

SHELL ["/bin/bash", "-c"]
CMD ["bash"]

