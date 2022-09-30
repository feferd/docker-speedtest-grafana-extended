FROM debian:11-slim AS get-speedtest

#case $(uname -m) in \
#  x86_64) export PROTOC_ZIP=protoc-${PROTOC_VERSION}-linux-x86_64.zip;; \
#  aarch64) export PROTOC_ZIP=protoc-${PROTOC_VERSION}-linux-aarch_64.zip;; \
#  *) echo "unsupported architecture"; exit 1 ;; \
#  esac; \
RUN apt-get update && apt-get install wget gnupg1 apt-transport-https dirmngr lsb-release -y
RUN wget https://install.speedtest.net/app/cli/ookla-speedtest-1.2.0-linux-$(uname -m).tgz -O /tmp/speedtest.tgz
RUN mkdir /tmp/speedtest && tar zxf /tmp/speedtest.tgz -C /tmp/speedtest && chmod +x /tmp/speedtest/speedtest


FROM alpine as install-dependencies
RUN apk add --no-cache npm
WORKDIR /build
COPY . .
RUN npm ci


FROM alpine as prod-stage
RUN apk add --no-cache nodejs
LABEL maintainer="Jonas Friedmann <j@frd.mn>"

WORKDIR /usr/src/app

CMD [ "node", "index.js" ]

COPY --from=get-speedtest /tmp/speedtest/speedtest /usr/bin/speedtest
COPY --from=install-dependencies /build .
