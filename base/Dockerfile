# -*- coding: utf-8 -*-
# vim: ft=Dockerfile

# aqua deploy Dockerfile - base
FROM node:10-alpine
LABEL maintainer="vkom <admin@vkom.cc>"

ARG MYTIME_RELEASE_URL=https://example.com

# prepare build directory:
RUN mkdir -pm0700 /usr/src/aqua

WORKDIR /usr/src/aqua

# download and unpack aqua release:
RUN apk add --no-cache curl \
	&& curl -LSs ${MYTIME_RELEASE_URL} | tar zxvC .

# install python for as npm plugin dependence:
RUN apk add --no-cache build-base python

# npm install:
RUN npm i

# remove package metadata files for production:
RUN rm -vf package.json \
	&& rm -vf package-lock.json

# fix permissions:
RUN apk add --no-cache findutils \
	&& find . -type f -exec chmod 0440 {} \; \
	&& find . -type d -exec chmod 2150 {} \; \
	&& apk del findutils

# prepare additional directories:
RUN mkdir -pm0170 cache

# debug:
RUN ls -la /usr/src/aqua
