# Dockerfile to build the image used to build the library.
FROM node:22-alpine

RUN apk --no-cache add make chromium

ENV CHROME_BIN=/usr/bin/chromium

WORKDIR /project
