FROM openpriv/flutter-desktop:linux-fstable-3.7.1

## cirruslabs/docker-images-android/sdk/33

# https://developer.android.com/studio/releases/build-tools
ENV ANDROID_PLATFORM_VERSION 33
ENV ANDROID_BUILD_TOOLS_VERSION 33.0.2

RUN yes | sdkmanager \
    "platforms;android-$ANDROID_PLATFORM_VERSION" \
    "build-tools;$ANDROID_BUILD_TOOLS_VERSION"



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

# Install Ruby based on https://www.geeksforgeeks.org/how-to-install-ruby-bundler-on-linux/
ENV BUILD_PACKAGES ruby-dev
ENV RUBY_PACKAGES \
  ruby

RUN apt-get install $BUILD_PACKAGES $RUBY_PACKAGES

RUN echo 'gem: --no-document' > /etc/gemrc && \
    gem install bundler

RUN bundle config --global silence_root_warning 1

# Install Chrome based on https://askubuntu.com/a/1206153; see https://stackoverflow.com/a/57731450/8848398
RUN echo 'deb [arch=amd64 signed-by=/usr/share/keyrings/debian-buster.gpg] http://deb.debian.org/debian buster main \n\
deb [arch=amd64 signed-by=/usr/share/keyrings/debian-buster-updates.gpg] http://deb.debian.org/debian buster-updates main \n\
deb [arch=amd64 signed-by=/usr/share/keyrings/debian-security-buster.gpg] http://deb.debian.org/debian-security buster/updates main' > /etc/apt/sources.list.d/debian.list \
    && sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys DCC9EFBF77E11517 \
    && sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 648ACFD622F3D138 \
    && sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 112695A0E562B32A \
    && sudo apt-key export 77E11517 | sudo gpg --dearmour -o /usr/share/keyrings/debian-buster.gpg \
    && sudo apt-key export 22F3D138 | sudo gpg --dearmour -o /usr/share/keyrings/debian-buster-updates.gpg \
    && sudo apt-key export E562B32A | sudo gpg --dearmour -o /usr/share/keyrings/debian-security-buster.gpg \
    && echo '# Note: 2 blank lines are required between entries \n\
Package: * \n\
Pin: release a=eoan \n\
Pin-Priority: 500 \n\
\n\
Package: * \n\
Pin: origin "deb.debian.org" \n\
Pin-Priority: 300 \n\
\n\
# Pattern includes "chromium", "chromium-browser" and similarly \n\
# named dependencies: \n\
Package: chromium* \n\
Pin: origin "deb.debian.org" \n\
Pin-Priority: 700' > /etc/apt/preferences.d/chromium.pref \
    && sudo apt update \
    && sudo apt install -y chromium
ENV CHROME_EXECUTABLE /usr/bin/chromium

# Solution taken from https://github.com/puppeteer/puppeteer/issues/5505#issuecomment-731933795
# Headless Chrome requires --no-sandbox in order to work in a Docker environment.
ENV CHROMIUM_FLAGS "--no-sandbox"