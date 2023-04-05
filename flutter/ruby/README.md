# Docker Nginx + PM2

Image based on Alpine

Uses:

- [nginx:1.23.3-alpine](https://github.com/nginxinc/docker-nginx/blob/5ce65c3efd395ee2d82d32670f233140e92dba99/mainline/alpine/Dockerfile) image,
- [Node.js v14.21.1](https://nodejs.org/en/blog/release/v14.21.1/) based on [this Dockerfile](https://github.com/nodejs/docker-node/blob/3f8018043408490439723ed3b71ab5578d69ea70/14/alpine3.16/Dockerfile)
- [git](https://pkgs.alpinelinux.org/packages?name=git&branch=edge&repo=main),
- [PM2](https://pm2.keymetrics.io/)