# -*- coding: utf-8 -*-
# vim: ft=Dockerfile

# aqua deploy Dockerfile - public
ARG AQUA_BASE_IMAGE="registry.is-docker.mh00p.net/aqua-base:latest"
ARG AQUA_SITE_DIR="/srv/aqua"


# fucking hacks for docker COPY --from =(
# https://github.com/moby/moby/issues/34482
FROM $AQUA_BASE_IMAGE as source


FROM alpine:latest
LABEL maintainer="vkom <admin@vkom.cc>"

# env for cmd exec:
ARG AQUA_SITE_DIR

# create site dir:
RUN mkdir -pm0111 $AQUA_SITE_DIR

WORKDIR $AQUA_SITE_DIR

# copy files:
COPY --chown=root:root --from=source /usr/src/aqua/ $AQUA_SITE_DIR

# fix permissions for anonymous users:
RUN chmod -R a=rX $AQUA_SITE_DIR \
	&& chattr -R +i $AQUA_SITE_DIR

# remove public and bot files:
RUN rm -vrf cache views node_modules app.js bot.js

# create log dir: XXX - log dir used only with forever
# RUN mkdir -pm0170 /var/log/aqua \
#	&& chgrp w3aqua /var/log/aqua

STOPSIGNAL SIGKILL

# run dummy cmd for public serve:
CMD /bin/rm -rfv /media/aqua ; /bin/cp -av /srv/aqua/. /media/aqua ; cat /dev/stdout
