FROM openfaas/of-watchdog:0.5.3 as watchdog

FROM node:10.12.0-alpine as ship

## Todo update versions

ENV APP_ROOT /home/app

COPY --from=watchdog /fwatchdog /usr/bin/fwatchdog
RUN chmod +x /usr/bin/fwatchdog

RUN npm install --save-dev @babel/core@~7.6.0 @babel/node@~7.6.1 @nuxt/babel-preset-app-edge

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

WORKDIR ${APP_ROOT}/

RUN rm -rf node_modules/
RUN npm i
RUN npm run build

ENV cgi_headers="true"
ENV fprocess="babel-node --presets=@nuxt/babel-preset-app-edge server.js"
ENV mode="http"
ENV upstream_url="http://127.0.0.1:3000"

ENV exec_timeout="20s"
ENV write_timeout="33s"
ENV read_timeout="33s"

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]