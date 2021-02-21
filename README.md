# Spack GitHub Container Registry

This repository is a testing bed to develop a spack container (using a Dockerfile 
generated with `spack containerize` against a spack.yaml) that can be triggered
to update with a [binoc](https://github.com/autamus/binoc) GitHub action
and then build and test the container, and if it works, push to a GitHub container
registry. We want to do the following:

 - [1. Minimize the size of the container](#minimize-container-size)
 - [2. Binoc GitHub Action](#binoc-github-action)
 - [3. Figure out how tests are added, and results are captured](#tests)
   - What percentage of spack packages have tests?
   - Can we capture and get unique identifiers for output?
 - [4. Add GitHub action to build, test, and capture results](#)
 - [5. Create static repository to hold results](#)
 - [6. Push container to GitHub registry (and test limits)](#)
  
## Overview

We have an idea that we would want to be able to:

1. configure a repository to regularly check for updates for some set of spack packages
2. given an update, open a pull request to build and test a container

And if the container build and tests are successful, we would want to push
the container to a registry. This requires the stages that are described here,
broadly:

## Stages

### Minimize Container Size

The original goal of this small test was to see if we can use a multi-stage
build to build binaries and then remove spack. This was the first time I tried this,
and to my surprise, spack already does this! See the [minimize-container](minimize-container)
folder for an overview.

### Binoc GitHub Action

At this point, we'd want to be able to add a folder of packages, and 
a GitHub Action (a workflow/*.yml file) to the repository, with an 
instruction to check for package updates at some frequency. When an update is
present, the binoc robot should open a pull request. The repo where
the action.yml should be added is [here](https://github.com/autamus/binoc).

**under development!** @vsoch will test when the GitHub action is ready.

### Tests

I'm actually not sure how spack represents tests. I know you can do an install with
them, e.g.,

```bash
$ spack install <package> --run-tests
```
But I'd like to know where/how that works, what kind of tests are run, how
output is stored (if at all) and how tests are namespaced. We would want to
understand this so that we can couple the container build with testing, to:

1. be sure that tests still work!
2. upload results to some testing repository (possibly using go-github) for future comparison.

### Resources

The following resources (things mentioned above) might be useful:

 - [https://github.com/arken/ait/blob/master/apis/github/files.go](https://github.com/arken/ait/blob/master/apis/github/files.go)
 - [https://github.com/google/go-github](https://github.com/google/go-github)
 - [https://github.com/autamus/docs](https://github.com/autamus/docs) as a place to write docs

