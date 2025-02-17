# Docker image file that describes an Alpine image with PowerShell and test dependencies

ARG BaseImage=mcr.microsoft.com/powershell:alpine-3.12

FROM node:14.17.6-alpine as node

# Do nothing, just added to borrow the already built node files.

FROM ${BaseImage}

ENV NODE_VERSION=14.17.6 \
    YARN_VERSION=1.22.5 \
    NVM_DIR="/root/.nvm" \
    POWERSHELL_DISTRIBUTION_CHANNEL=PSDocker-TestDeps-Alpine-3.12

# workaround for Alpine to run in Azure DevOps
ENV NODE_NO_WARNINGS=1

# Copy node and yarn into image
COPY --from=node /usr/local/bin/node /usr/local/bin/node
COPY --from=node /opt/yarn-v${YARN_VERSION} /opt/yarn-v${YARN_VERSION}

RUN apk add --no-cache --virtual .pipeline-deps readline linux-pam \
    && apk add \
        bash \
        sudo \
        shadow \
        openssl \
        curl \
    && apk del .pipeline-deps \
    && ln -s /opt/yarn-v$YARN_VERSION/bin/yarn /usr/local/bin/yarn \
    && ln -s /opt/yarn-v$YARN_VERSION/bin/yarnpkg /usr/local/bin/yarnpkg

LABEL com.azure.dev.pipelines.agent.handler.node.path="/usr/local/bin/node"

# Use PowerShell as the default shell
# Use array to avoid Docker prepending /bin/sh -c
CMD [ "pwsh" ]
