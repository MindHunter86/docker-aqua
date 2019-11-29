# -*- coding: utf-8 -*-
# vim: ft=Dockerfile

# aqua deploy Dockerfile - bot
ARG AQUA_BASE_IMAGE="registry.is-docker.mh00p.net/aqua-base:latest"
ARG AQUA_SITE_DIR="/srv/aqua"
ARG AQUA_BOT_ID=1


# fucking hacks for docker COPY --from =(
# https://github.com/moby/moby/issues/34482
FROM $AQUA_BASE_IMAGE as source


FROM node:12-alpine
LABEL maintainer="vkom <admin@vkom.cc>"

# env for cmd exec:
ARG AQUA_SITE_DIR
ENV AQUA_SITE_DIR $AQUA_SITE_DIR

ARG AQUA_BOT_ID
ENV AQUA_BOT_ID $AQUA_BOT_ID

# create aqua user/group:
RUN addgroup --system w3aqua \
	&& adduser --system --disabled-password --home "$AQUA_SITE_DIR" --shell /sbin/nologin --ingroup w3aqua w3aqua

# create site dir:
RUN mkdir -pm0110 $AQUA_SITE_DIR \
	&& chgrp w3aqua $AQUA_SITE_DIR

WORKDIR $AQUA_SITE_DIR

# copy files:
COPY --chown=root:w3aqua --from=source /usr/src/aqua/ $AQUA_SITE_DIR

# remove public and bot files:
RUN rm -vrf public cache views app.js

# AQUA_SITE_DIR files listing for debugging:
RUN ls -la $AQUA_SITE_DIR

# set user for next CMD instruction:
USER w3aqua:w3aqua

# run bot: 
CMD node ${AQUA_SITE_DIR}/bot.js $AQUA_BOT_ID
