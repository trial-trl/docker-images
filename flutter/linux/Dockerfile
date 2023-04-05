FROM openpriv/flutter-desktop:linux-fstable-3.7.1

ENV PATH="$PATH":"/root/.pub-cache/bin"
RUN dart pub global activate fvm \
    && fvm --version

# Install node.js based on https://github.com/nodejs/docker-node/blob/main/16/buster-slim/Dockerfile
ENV NODE_MAJOR_VERSION 16
ENV NODE_VERSION 16.20.0

# Install Node.js, Yarn and required dependencies
RUN apt-get update \
  && apt-get install -y curl gnupg build-essential \
  && curl --silent --location https://deb.nodesource.com/setup_${NODE_MAJOR_VERSION}.x | bash - \
  && curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
  && echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list \
  && apt-get remove -y --purge cmdtest \
  && apt-get update \
  && apt-get install -y nodejs yarn \
  # remove useless files from the current layer
  && rm -rf /var/lib/apt/lists/* \
  && rm -rf /var/lib/apt/lists.d/* \
  && apt-get autoremove -y \
  && apt-get clean \
  && apt-get autoclean \
  && corepack enable