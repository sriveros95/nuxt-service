FROM openfaas/of-watchdog:0.7.2 as watchdog

FROM node:10.16-slim as ship

## Todo update versions

ENV APP_ROOT /home/app

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog


RUN apt-get update; \
    apt-get install -y ca-certificates; \
    rm -rf /var/lib/apt/lists/*

WORKDIR /root/

# Turn down the verbosity to default level.
ENV NPM_CONFIG_LOGLEVEL warn

RUN mkdir -p ${APP_ROOT}

# Wrapper/boot-strapper
WORKDIR ${APP_ROOT}
COPY --chown=node:node function/app/ ./
COPY --chown=node:node package.json ./

## use/enable node user
USER node

WORKDIR ${APP_ROOT}/


RUN rm -rf node_modules/ ; rm package-lock.json; npm i

# COPY function node packages and install, adding this as a separate
# entry allows caching of npm install
COPY function/app/*.json ./
RUN npm i || :

RUN npm run build

#ENV cgi_headers="true"
ENV fprocess="npm start"
ENV mode="http"
ENV upstream_url="http://127.0.0.1:3000"
EXPOSE 3000
#ENV exec_timeout="10s"
#ENV write_timeout="15s"
#ENV read_timeout="15s"

USER root
# chmod for tmp is for a buildkit issue (@alexellis)
RUN chmod 777 /tmp
USER node

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]