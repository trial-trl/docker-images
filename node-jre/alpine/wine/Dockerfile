FROM mlsm/node-jre:node24.15.0-jre17-alpine3.23

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