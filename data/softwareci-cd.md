# CI/CD Pipelines

## Introduction

CI/CD automates testing and deployment. This tutorial covers setting up CI/CD pipelines with GitHub Actions.

---

## GitHub Actions

    name: CI
    on: [push]
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v2
          - run: npm install
          - run: npm test

---

## Conclusion

Automate testing and deployment with CI/CD. Use GitHub Actions, Jenkins, or other CI/CD tools.

