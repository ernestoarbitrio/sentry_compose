Sentry docker-compose
--------

This is the docker-compose file that we use at [artbio](http://artbio.fr) to keep
track of our galaxy servers https://lbcd41.snv.jussieu.fr and https://mississippi.snv.jussieu.fr

Images
-------

We're using the official sentry, redis, nginx and postgres images
to setup an instance of sentry (including celery workers and celery beat).

Configuration
-------


**IMPORTANT**

We excluded the secret_env_vars file from this repository.
You can create your own, its content must contain the secret key that is shared by all sentry containers.
To generate the secret key, run
```
docker run --rm sentry generate-secret-key`
```
You can write this secret key into the secret_env_vars file,
that will be read when running docker-compose up:
```
SENTRY_SECRET_KEY=<YOUR SECRET KEY HERE>
```

We also included the password for our email account in this file:

```
SENTRY_EMAIL_PASSWORD=<YOUR TOP SECRET EMAIL PASSWORD>
```

Adapt the location of you persistent file location in the data service in docker-compose.yml.
By default, we only expose nginx on port 80 that proxies sentry.
Make any changes to the supplied nginx.conf, which will be mounted into the nginx container.
Similaly, changes to sentry.conf.py will be applied upon rebuilding the sentry image.
(`docker-compose build`).

First start
--------

Unfortunately we need to enter some manual configuration upon first starting sentry.
Use 
```
docker-compose run sentry bash
```

to startup postgres, redis and and sentry. Inside the sentry container type sentry upgrade.
This will migrate the sentry database, and at the end you can create a superuser account.

Ypu can then start all required images by typing
```
docker-compose up -d
```

Since we set restart=always in the docker compose file, sentry will start up as soon as docker is started.
