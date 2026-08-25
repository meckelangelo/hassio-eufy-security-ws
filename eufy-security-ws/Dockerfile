ARG BUILD_FROM
FROM $BUILD_FROM

ARG EUFY_SECURITY_WS_VERSION

WORKDIR /usr/src/app

RUN echo "T8426 BUILD v2"

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
    && npm install --force ./eufy-security-client.tgz \
    && echo "=== BEFORE REMOVING NESTED CLIENT ===" \
    && node -e "const p=require.resolve('eufy-security-client/package.json',{paths:['/usr/src/app/node_modules/eufy-security-ws']}); console.log('Resolved:',p); console.log('Version:',require(p).version)" \
    && rm -rf /usr/src/app/node_modules/eufy-security-ws/node_modules/eufy-security-client \
    && echo "=== AFTER REMOVING NESTED CLIENT ===" \
    && node -e "const p=require.resolve('eufy-security-client/package.json',{paths:['/usr/src/app/node_modules/eufy-security-ws']}); console.log('Resolved:',p); console.log('Version:',require(p).version)" \
    && rm -rf /tmp/eufy-security-client

COPY run.sh /
RUN chmod a+x /run.sh

WORKDIR /data

CMD [ "/run.sh" ]
