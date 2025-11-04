FROM ruby:3.1

# Install essentials
RUN apt-get update && apt-get install -y \
    build-essential \
    zlib1g-dev \
    nodejs \
    && rm -rf /var/lib/apt/lists/*

# Install Jekyll + Bundler
RUN gem install bundler jekyll

WORKDIR /workspace
