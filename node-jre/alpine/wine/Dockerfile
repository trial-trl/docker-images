FROM node:14.21.2-alpine3.16

ARG JAVA_VERSION=8.352.08.1

RUN wget -O /THIRD-PARTY-LICENSES-20200824.tar.gz https://corretto.aws/downloads/resources/licenses/alpine/THIRD-PARTY-LICENSES-20200824.tar.gz && \
    echo "82f3e50e71b2aee21321b2b33de372feed5befad6ef2196ddec92311bc09becb  /THIRD-PARTY-LICENSES-20200824.tar.gz" | sha256sum -c - && \
    tar x -ovzf THIRD-PARTY-LICENSES-20200824.tar.gz && \
    rm -rf THIRD-PARTY-LICENSES-20200824.tar.gz && \
    wget -O /etc/apk/keys/amazoncorretto.rsa.pub https://apk.corretto.aws/amazoncorretto.rsa.pub && \
    SHA_SUM="6cfdf08be09f32ca298e2d5bd4a359ee2b275765c09b56d514624bf831eafb91" && \
    echo "${SHA_SUM}  /etc/apk/keys/amazoncorretto.rsa.pub" | sha256sum -c - && \
    echo "https://apk.corretto.aws" >> /etc/apk/repositories && \
    apk add --no-cache amazon-corretto-8-jre=$JAVA_VERSION-r0 && \
    # smoke test
    java -version && \
    # add basic tools missing in alpine
    apk add --no-cache git
    
ENV LANG C.UTF-8
ENV JAVA_HOME=/usr/lib/jvm/default-jvm/jre

RUN corepack enable

# add Wine
ENV WINEDEBUG -all,err+all
ENV WINEDLLOVERRIDES winemenubuilder.exe=d
ENV WINEPREFIX /root/wine64
ENV WINEARCH win64
ENV DISPLAY :0
RUN apk add --no-cache wine \
    # config Wine
    && winecfg \
    # create symlink to default wine
    && ln -sf /usr/bin/wine64 /usr/bin/wine \
    # smoke test
    && wine --version \
    # cleanup
    && rm -rf /var/cache/apk/*