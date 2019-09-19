OpenFaaS vue/nuxt micro-service template
=============================================
WIP
status: there is hope

Valuable knowledge: [A Dockerized SSR runtime to develop, run, and deploy your frontend webapp. by thegreenhouseio](https://github.com/thegreenhouseio/docker-ssr)

This template provides vue/nuxt and full access to [express.js](http://expressjs.com/en/api.html#req.is) for building microservices for [OpenFaaS](https://www.openfaas.com), Docker, Knative and Cloud Run.

With this template you can create a new microservice and deploy it to a platform like [OpenFaaS](https://www.openfaas.com) for:

* scale-to-zero
* horizontal scale-out
* metrics & logs
* automated health-checks
* sane Kubernetes defaults like running as a non-root user

## Status of the template

This template is experimental and I would like your feedback through GitHub issues or [OpenFaaS Slack](https://docs.openfaas.com/community).

## Supported platforms

* x86_64 - `nuxt-service`

## Get started

You can create or scaffold a new microservice using the [OpenFaaS CLI](https://github.com/openfaas/faas-cli).

```
# USERNAME is your Docker Hub account or private Docker registry
$ export USERNAME=alexellisuk

$ faas template pull https://github.com/sriveros95/nuxt-service
$ faas new --lang nuxt-service microservice1 --prefix="${USERNAME}"
```

The idea is that one only needs to put the files needed to build the app in the app folder.

Move/Merge package.json out of app folder into nuxt-service folder.


*handler.js*
