# Seldon Core Operator OCI Image

This repository is intended as a demonstration of how to port an OCI image that it otherwise
defined using a `Dockerfile` to one built with [rockcraft](https://github.com/canonical/rockcraft).

It provides a direct port of this [Dockerfile](https://github.com/SeldonIO/seldon-core/blob/5591c42b6a40a44641b848d86f9228f623c64598/operator/Dockerfile).

Critically, this image includes [pebble](https://github.com/canonical/pebble) to start and manage
the Seldon Core operator.

## Build steps

```bash
# Install some deps
$ sudo snap install rockcraft --classic --edge
$ sudo snap install skopeo --edge --devmode

# Pack the ROCK (optionally add --verbose)
$ rockcraft pack

# For the following commands, you must either use `sudo` or ensure your user is a member of the
# `docker` group, or the commands will fail.

# Import the ROCK into the local docker image cache
$ sudo skopeo --insecure-policy copy oci-archive:seldon-core-operator_1.14.1_amd64.rock docker-daemon:jnsgruk/seldon-core-operator:1.14.1

# Run the image, invoking `manager` through pebble
$ docker run --rm -it jnsgruk/seldon-core-operator:1.14.1
```

## TODO

- [ ] Validate if the YAML resources in `/tmp/operator-resources` are required
- [x] Test with an actual charm deployment
- [ ] Fine-tune the default Pebble layer
- [x] Run the operator as a non-root user
- [ ] Trim the default image size using [chiselled ubuntu](https://github.com/canonical/chisel)
