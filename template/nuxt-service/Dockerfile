FROM openfaas/of-watchdog:0.5.3 as watchdog

FROM node:10.12.0-alpine as ship

## Todo update versions

ENV APP_ROOT /home/app

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN apk --no-cache add ca-certificates

WORKDIR /root/

# Turn down the verbosity to default level.
ENV NPM_CONFIG_LOGLEVEL warn

RUN mkdir -p ${APP_ROOT}

# Wrapper/boot-strapper
WORKDIR ${APP_ROOT}
COPY --chown=node:node function/app/ ./
COPY --chown=node:node package.json ./

# chmod for tmp is for a buildkit issue (@alexellis)
RUN chmod 777 /tmp

## use/enable node user
USER node

# This ordering means the npm installation is cached for the outer function handler.
RUN npm i
RUN npm run build
RUN npm run start

# Copy outer function handler
#COPY index.js ./

# COPY function node packages and install, adding this as a separate
# entry allows caching of npm install
##WORKDIR ${APP_ROOT}/function
##COPY function/*.json ./
##RUN npm i || :

# COPY function files and folders
##COPY function/ ./

WORKDIR ${APP_ROOT}/

ENV cgi_headers="true"
ENV fprocess="node .nuxt/server.js"
ENV mode="http"
ENV upstream_url="http://127.0.0.1:3000"

ENV exec_timeout="10s"
ENV write_timeout="15s"
ENV read_timeout="15s"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]