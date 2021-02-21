# Spacktract (Spack Extract)

This folder is a testing ground to develop a spack container (using a Dockerfile 
generated with `spack containerize` against a spack.yaml) that can be triggered
to update with a [binoc](https://github.com/autamus/binoc) GitHub action
and then build and test the container, and if it works, push to a GitHub container
registry.

## Steps

### 1. Create Testing Container
First, we create a [spack.yaml](spack.yaml) that defines an environment with packages.
We then generate a Dockerfile with containerize:

```bash
$ spack containerize
```

We can then build to see the size of the container.

```bash
$ docker build -t trilinos .
```
```bash
$ docker images | grep trilinos
trilinos                                latest    3a5abcb21b12   About an hour ago   639MB
```

### 2. Find Binaries

We can next shell into the container:

```bash
$ docker run -it trilinos
```
Interesting! So spack already created a view for trilinos for us:

```bash
/opt/
|-- software
|-- spack-environment
`-- view
```

Oh wow! So I was [reading about views](https://spack.readthedocs.io/en/latest/workflows.html#filesystem-views),
(and planning to use this approach) but after viewing the above, I looked at the Dockerfile, and *we
are already doing a multi-stage build to remove most of spack* except for what we need. No work needs
to be done here! So let's have this repository be a testing ground for setting up a GitHub action,
as discussion in the above section.
