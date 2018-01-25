# CiviCRM buildkit on Docker

A buildkit oriented docker set up for CiviCRM. This repository creates a CiviCRM development environment based on buildkit.

It tries to follow the Docker principles of isolating services in containers (rather than munging the whole set up into a single container) in order to make it easier to experiment with different versions and flavours of the various services.

* `nginx` from nginx: serves the development sites
* `fpm` from php-fpm: processes php requests from nginx
* `mysql` from mysql: the databases
* `cli` - from php-cli: for the build process *

\* `cli` also includes nodejs and a few other utilities.

The `civicrm-buildkit/build` directory is bind mounted at `./build` for local development.

## Getting started

1. Install Docker and Docker compose
2. Clone this repository
3. Start the containers with `docker-compose up -d`
4. Create a dmaster build with `docker-compose exec cli civibuild create dmaster`
5. The build will be available at `./build/dmaster`

## Tips

### Browsing sites

`civibuild` runs in a container and cannot update `/etc/hosts` on the hosts machine - you need to manually configure a `/etc/hosts` entry. `civibuild` is configured with the following URL_TEMPLATE: `http://%SITE_NAME%.buildkit:8080`. port 8080 on nginx is forwards to 8080 on the host. To access a site from the local machine add an entry to `/etc/hosts` along the lines of `127.0.0.1 %SITE_NAME%.buildkit`.

# Next steps

* Remove need to set up aliases, etc. on the host. Might require a rethink / new method for creating sites with buildkit
* Configure maildev for better mail testing.
* Decide on whether we should use nginx or apache (giving the option of either seems a bit wrong if we are trying to standardise on a stack)