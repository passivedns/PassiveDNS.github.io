---
title: CI/CD
prev: docs/architecture/docker
next: docs/authors
---

This section is about the Continuous Integration - Continuous Development setup for this application.

## Tests

Every time there is a Pull Request on the main branch of the project, a set of tests are going to run, to check the integrity of the model and the requests. The tests are located in the `tests` folder.
See `.github/workflows/tests.yml` for more details.


## Release

Every time there is a release, a set of images from each container is going to be built and pushed for the production environment.
See `.github/workflows/main.yml` for more details.