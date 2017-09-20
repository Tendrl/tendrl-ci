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
* *Tendrl build: ...* jobs - build particular package
* *Tendrl pkgval: ...* jobs - package validation tests
* *Tendrl test: ...* jobs - run tests
* auxiliary jobs - prepare build and testing environment, build all packages at once

### relase branch
Dashboard view: https://ci.centos.org/view/Tendrl

List of jobs (detailed view): https://ci.centos.org/view/tendrl-build-release/


### master branch
Dashboard view: https://ci.centos.org/view/Tendrl-master/

List of jobs (detailed view): https://ci.centos.org/view/tendrl-build-master/

## Detailed description

Using CentOS CI have some specifics and some parts of the building process is
not as straight forward as it might be.
Main "problem" (specific) is, that we are not the owners/admins of the Jenkins.
And the Jenkins slave where are running our jobs is shared with other projects,
so we have not rights to install there anything and we should not break anything.

The package trigger/build process is not performed directly on Jenkins slave
(the reason was described in previous paragraph), but on ad-hoc prepared machine
borrowed from [CentOS CI/Duffy](https://wiki.centos.org/QaWiki/CI/Duffy).

### Trigger/Build process

When job *Tendrl build: (\<branch-name\>) - \<package-name\>* is triggered
  (by GitHub Webhook or manually):

* check, if relevant *build_server* is available and ready
  * if not, launch 	*Tendrl_build_server (\<branch-name\>) prepare* job
    and wait till *build_server* is ready
* on *build_server*:
  - clone the relevant git repository
  - checkout desired branch
  - create *srpm* package using appropriate `make ...` command
  - submit the *srpm* package to Copr using `copr-cli build ...`
* run *Tendrl_build_server (\<branch-name\>) teardown* job
  - check, if *build_server* is not used by some other job and return it back to the pool

Detailed description of CentOS CI is out of scope for this document.
Feel free to raise any question or check official documentation:
https://wiki.centos.org/QaWiki/CI
https://wiki.centos.org/QaWiki/CI/GettingStarted

## How to configure the jobs?

* configure Jenkins Job Builder (JJB) - see example configuration `jobs/jenkins-config.example.ini`
* update the job configuration files in `jobs/build-triggers/`
* update the jobs:

  ``jenkins-jobs --ignore-cache --flush-cache --conf jobs/jenkins-config.ini -l debug update jobs/build-triggers/``
