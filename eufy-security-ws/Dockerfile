ARG BUILD_FROM
FROM $BUILD_FROM

ARG EUFY_SECURITY_WS_VERSION

WORKDIR /usr/src/app
RUN \
    set -x \
    # The Alpine 3.23 base image (see build.yaml) ships Node.js 24 in its main
    # repository, so nodejs/npm install directly with no edge-repo workarounds.
    && apk add --no-cache \
        jq \
        git \
        nodejs \
        npm \
    && node --version \
    # Fail the build if the base ever regresses below Node 24.
    && case "$(node --version)" in v24.*) ;; *) echo "Expected Node 24, got $(node --version)" >&2; exit 1 ;; esac \
    && npm install --force \
    "eufy-security-ws@${EUFY_SECURITY_WS_VERSION}"

ARG BUILD_FROM
FROM $BUILD_FROM

ARG EUFY_SECURITY_WS_VERSION

WORKDIR /usr/src/app

RUN \
    set -x \
    && apk add --no-cache \
        git \
        jq \
        nodejs \
        npm \
    && node --version \
    && case "$(node --version)" in v24.*) ;; *) echo "Expected Node 24, got $(node --version)" >&2; exit 1 ;; esac \
    && npm install --force \
        "eufy-security-ws@${EUFY_SECURITY_WS_VERSION}"

# Install patched T8426 client
RUN \
    set -x \
    && git clone \
        --branch t8426-light-fix \
        --depth 1 \
        https://github.com/meckelangelo/eufy-security-client.git \
        /tmp/eufy-security-client \
    && cd /tmp/eufy-security-client \
    && npm ci \
    && npm run build \
    && npm pack \
    && mv eufy-security-client-*.tgz /usr/src/app/eufy-security-client.tgz \
    && cd /usr/src/app \
    && npm pkg set overrides.eufy-security-client=file:./eufy-security-client.tgz \
    && npm install --force \
    && rm -rf /tmp/eufy-security-client

COPY run.sh /
RUN chmod a+x /run.sh

WORKDIR /data

CMD [ "/run.sh" ]

COPY run.sh /
RUN chmod a+x /run.sh
WORKDIR /data

CMD [ "/run.sh" ]
