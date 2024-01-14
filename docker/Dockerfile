FROM --platform=$BUILDPLATFORM php:8.1-apache-bullseye as builder

CMD ["apache2-foreground"]

FROM builder as dev-envs

RUN <<EOF
apt-get update
apt-get install -y --no-install-recommends git
EOF

RUN <<EOF
useradd -s /bin/bash -m vscode
groupadd docker
chown www-data:www-data html
chown -R www-data:www-data html/src/cache html/src/log html/src/config
chmod -R 775 html/src/cache html/src/log html/src/config 
usermod -aG docker vscode
EOF
# install Docker tools (cli, buildx, compose)
COPY --from=gloursdocker/docker / /

CMD ["apache2-foreground"]