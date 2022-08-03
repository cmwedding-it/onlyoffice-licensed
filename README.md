# Patched ONLYOFFICE Docs (Community Edition)

[![Docker Image CI](https://github.com/cmwedding-it/onlyoffice-licensed/actions/workflows/docker-image.yml/badge.svg)](https://github.com/cmwedding-it/onlyoffice-licensed/actions/workflows/docker-image.yml)

## Notes

Please don't ask for setup support. I cannot support all your different setups. Sorry.
Please also don't ask OnlyOffice devs for support if something is broken with this image.

**There will be bugs.**
**Some functionaly will absolutely be broken.**
**Don't expect this repository to provide every feature the paid version provides.**

**Some mobile editing features are reverted by the patches to very old code (before they were removed).**
**This code can not only break but also will not receive security updates.**

## Features

This [Dockerfile](./Dockerfile) and patches compile a version of
OnlyOffice Docs server with mobile editing enabled in the Nextcloud apps for an
unlimited amount of concurrent users.

The patches provided in this repository only restores basic editing on mobile.

It can be integrated into e.g. Nextcloud or ownCloud like the official images.

## Background

Just about two months after [Nextcloud released their partnership with Ascensio](https://nextcloud.com/blog/onlyoffice-and-nextcloud-partnering-up/)
and featured a community version of OnlyOffice, the latter decided to remove
support for mobile editing of documents. This affected the Nextcloud app,
killing a feature that was previously marketed by both companies.

The changes were executed without any prior notice and alienated quite a lot of
home users, who would now be forced to pay more than €1.000 to unlock that
previously free feature. Only after some outcries Ascensio deigned to release a
statement and a new, albeit "limited", offer of €90 for home servers. This
offer has since expired and their licensing tier suggests current licenses are
valid for one year, starting at about €140.

In my opinion these deceptive practices of advertising a feature only to take
it away are unacceptable for a company presenting itself and their products as
open source.


## Usage

Please refer the the official docs on how to integrate OnlyOffice into your
setup.

### Podman CLI

```sh
podman run \
    --name=onlyoffice \
    --detach \
    --publish=80:80 \
    docker.io/cmwedding/onlyoffice-licensed
```

### Docker CLI

```sh
docker run \
    --name=onlyoffice \
    --detach \
    --publish=80:80 \
    cmwedding/onlyoffice-licensed
```

### [docker-compose.yml](./docker-compose.yml)

```yml
services:
  onlyoffice:
    container_name: onlyoffice
    image: cmwedding/onlyoffice-licensed
    ports:
      - "80:80"
```

### Verify

To verify that the container is running successfully open
`[server-url]/healthcheck` (has to return `true`) and for the version number open
`[server-url]/index.html` and check the output of that page or
`[server-url]/web-apps/apps/api/documents/api.js` and check the header comment.


## Build

### Buildah CLI

```sh
buildah build-using-dockerfile \
    --tag=onlyoffice-patched \
    https://github.com/cmwedding/onlyoffice-licensed.git
```

### Docker CLI

```sh
docker build \
    --tag=onlyoffice-patched \
    https://github.com/cmwedding/onlyoffice-licensed.git
```


### docker-compose.yml

```yml
services:
  onlyoffice:
    container_name: onlyoffice
    image: onlyoffice-patched
    build:
      context: https://github.com/cmwedding/onlyoffice-licensed.git
    …
```


## Thanks

This is a fork of a fork of [aleho/onlyoffice-ce-docker-license](https://github.com/aleho/onlyoffice-ce-docker-license). The original repo was heavily inspired by the works of
[Zegorax/OnlyOffice-Unlimited](https://github.com/Zegorax/OnlyOffice-Unlimited).
