# Basics
The following environment variables need to be set, possibly in a file called `.env`:

```
ACME_EMAIL=postmaster@example.com
TRAEFIK_API_FQDN=traefik.example.com
TRAEFIK_API_CREDENTIALS=test:$apr1$H6uskkkW$IgXLP6ewTrSuBkTrqE8wj/
```

The credentials are in `user:hash` format from `htpasswd`. And, of course, there are
docker images to provide `htpasswd` so that you don't need to find it separately.

For example:
`docker run --rm xmartlabs/htpasswd user password`

**Note:** in an environment file, you do not need to double dollar signs in the password
hash. If you put credentials directly in `docker-compose.yml` then remember to double
your dollar signs. See [BasicAuth Middleware](https://docs.traefik.io/middlewares/basicauth/)
documentation for more details.
