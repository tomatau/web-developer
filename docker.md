# Docker

## Builder Pattern

- 2 docker images
  - perform a build
  - ship results without penalty of build-chain

**Example**

1. Base image with build deps
2. Add source code
3. Produce binary
4. Copy binary from image to host
5. Derive light image with the binary from host

**Example for nodejs**

1. Derive *node base* FROM with `node` and `npm`
2. Add `package.json`
3. Install modules with deps and devDeps
4. Copy application code
5. Run build, code coverage, linters, tests
6. Create production image, FROM *node base*
7. Install node_modules `--only=production`
8. Expose PORT, define default CMD
9. Push production image to registry

## ONBUILD

- trigger instructions to execute at a later time
  - when used as the base for another build
  - in context of downstream build, as if immediately after FROM

```Dockerfile
# tomatao:breko-hub_builder
FROM node:x

WORKDIR /project/

ONBUILD COPY ./package.json /project/
ONBUILD RUN npm ci

CMD ["node", "."]
```

## ONBUILD and MULTISTAGE

- reuse images and the ONBUILD steps
- onbuild can reference base images

```Dockerfile
#
FROM tomatao:breko-hub_builder as builder

FROM debian:jessie
WORKDIR /project/

ONBUILD COPY --from=builder /usr/local/bin/node /usr/local/bin/
ONBUILD COPY --from=builder /usr/lib/ /usr/lib/
ONBUILD COPY --from=builder /project/ /project/
ONBUILD RUN npm ci

CMD ["node", "."]
```

**Double FROM**

- can stack ONBUILD and reference other base images
  - can have images for installing dependencies
  - another image for setting up minimal runtime

```Dockerfile
FROM tomatao:breko-hub_builder AS builder
# runtime copies files
FROM tomatao:breko-hub_runtime
```
