Tendrl CI
=========

[Tendrl](http://tendrl.org/) CI jobs and related documentation.

## Overview
Tendrl packages are built in [Fedora Copr](https://copr.fedorainfracloud.org/coprs/tendrl/),
triggered by respective jobs in [CentOS CI](ci.centos.org).

The jobs in CentOS CI are configured via Jenkins Job Builder
and this repository contains the respective configuration files
(in [jobs/build-triggers/](https://github.com/Tendrl/tendrl-ci/tree/master/jobs/build-triggers)
directory).

There are two groups of jobs:
* for packages from ``release/*`` branch
* for packages from ``master`` branch

Each group contains few types of jobs:
* *Tendrl build: \** jobs - build particular package
* *Tendrl pkgval: \** jobs - package validation tests
* *Tendrl test: \** jobs - run tests
* auxiliary jobs - prepare build and testing environment, build all packages at once

# relase branch
Dashboard view: https://ci.centos.org/view/Tendrl

List of jobs (detailed view): https://ci.centos.org/view/tendrl-build-release/


# master branch
Dashboard view: https://ci.centos.org/view/Tendrl-master/

List of jobs (detailed view): https://ci.centos.org/view/tendrl-build-master/

## How to configure the build jobs?

TBD

## Detailed description of the build process

TBD
