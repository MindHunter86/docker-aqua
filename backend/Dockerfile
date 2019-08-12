# -*- coding: utf-8 -*-
# vim: ft=Dockerfile

# aqua deploy Dockerfile - backend
ARG AQUA_BASE_IMAGE="registry.is-docker.mh00p.net/aqua-base:latest"
ARG AQUA_SITE_DIR="/srv/aqua"


# fucking hacks for docker COPY --from =(
# https://github.com/moby/moby/issues/34482
FROM $AQUA_BASE_IMAGE as source


FROM node:10-alpine
LABEL maintainer="vkom <admin@vkom.cc>"

# env for cmd exec:
ARG AQUA_SITE_DIR
ENV AQUA_SITE_DIR $AQUA_SITE_DIR

# create aqua user/group:
RUN addgroup --system w3aqua \
	&& adduser --system --disabled-password --home "$AQUA_SITE_DIR" --shell /sbin/nologin --ingroup w3aqua w3aqua

# create site dir:
RUN mkdir -pm2110 $AQUA_SITE_DIR \
	&& chgrp w3aqua $AQUA_SITE_DIR

WORKDIR $AQUA_SITE_DIR

# copy files:
COPY --chown=root:w3aqua --from=source /usr/src/aqua/ $AQUA_SITE_DIR

# fix permissions after COPY for site dir:
RUN chmod 2110 $AQUA_SITE_DIR

# remove public and bot files:
RUN rm -vrf public bot.js

# messy code for nodejs backend index.html err fix
# RUN mkdir -vm 2110 public \
# 	&& touch -m 0440 public/index.html

# AQUA_SITE_DIR files listing for debugging:
RUN ls -la $AQUA_SITE_DIR

# install backend requirements:
# RUN apk add --no-cache musl

# install tzdata and set Moscow timezone:
RUN apk add --no-cache tzdata \
	&& ln -s /usr/share/zoneinfo/Europe/Moscow /etc/localtime \
	&& echo 'Europe/Moscow' > /etc/timezone

# expose backend port:
EXPOSE 8080

# set user for next CMD instruction:
USER w3aqua:w3aqua

# run backend: 
CMD node ${AQUA_SITE_DIR}/app.js
