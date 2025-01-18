FROM mlsm/node-jre:node22.12.0-jre17-alpine3.21

# add Wine
ENV WINEDEBUG -all,err+all
ENV WINEDLLOVERRIDES winemenubuilder.exe=d
ENV WINEPREFIX /root/wine64
ENV WINEARCH win64
ENV DISPLAY :0
RUN apk add --no-cache wine \
    # config Wine
    && winecfg \
    # smoke test
    && wine --version \
    # cleanup
    && rm -rf /var/cache/apk/*