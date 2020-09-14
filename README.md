# docker-compose
Various configurations for Docker Compose projects

The projects with *traefik* in the name are intended to be used in
environments where the project in the base `traefik` directory is
also running.

The projects also assume environment variables are placed in a file
called `.env` in each project directory next to `docker-compose.yml`,
but these variable files have been excluded from git. The documentation
for each project should note which environment variables are expected,
but `docker-compose config` will also warn about missing variables:

```
$ docker-compose -f traefik/docker-compose.yml config
WARNING: The ACME_EMAIL variable is not set. Defaulting to a blank string.
WARNING: The TRAEFIK_API_CREDENTIALS variable is not set. Defaulting to a blank string.
WARNING: The TRAEFIK_API_FQDN variable is not set. Defaulting to a blank string.
```

* [Use Compose in production](https://docs.docker.com/compose/production/)
* [Multiple Compose files](https://docs.docker.com/compose/extends/)

A shell script or some aliases in this base directory would probably
help make it easier to work with this structure.
