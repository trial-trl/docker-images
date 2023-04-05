FROM openpriv/flutter-desktop:windows-sdk30-fstable-3.7.1

ENV NODE_VERSION 16.20.0

# Install scoop based on https://github.com/ScoopInstaller/Install#for-admin
RUN powershell.exe -Command \
    Set-ExecutionPolicy RemoteSigned -Scope CurrentUser; \
    irm get.scoop.sh -outfile 'install.ps1'; \
    ./install.ps1 -RunAsAdmin; \
    scoop install nodejs@$env:NODE_VERSION; \
    scoop install yarn;

# Update PATH based on https://github.com/docker-library/golang/blob/master/Dockerfile-windows-nanoserver.template
USER ContainerAdministrator
RUN setx /m PATH "C:\Users\ContainerAdministrator\AppData\Local\Pub\Cache\bin;C:\Program Files\Git\bin;%PATH%"

RUN dart pub global activate fvm \
    && fvm --version