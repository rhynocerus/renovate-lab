# Renovate Lab

Controlled laboratory for learning and documenting automated dependency management with Renovate and GitHub.

## Purpose

This repository is intentionally isolated from my active development projects. It provides a disposable test fixture where Renovate can discover outdated dependencies, propose configuration, and create update pull requests without affecting production or portfolio code.

## Current experiment

The lab contains a minimal Node.js fixture with deliberately outdated dependencies. The goal is to observe the complete Renovate workflow:

1. dependency discovery;
2. onboarding configuration;
3. Dependency Dashboard behavior;
4. patch, minor, and major update proposals;
5. generated pull requests;
6. manual review and risk assessment.

## Safety rules

- The repository remains private during the initial experiment.
- Renovate will be granted access only to this repository.
- Automatic merging is disabled during the pilot.
- No production secrets or application source code belong here.
- Docker is not required on the development workstation for this experiment.

## Test fixture

`package.json` is marked with `"private": true` to prevent accidental npm publication.

Test dependencies are intentionally behind current releases so that Renovate has update candidates to detect.

## Documentation

See [`docs/LAB-REPORT.md`](docs/LAB-REPORT.md) for the experiment log, safety model, phases, and conclusions.

## Status

🧪 Lab prepared. Next step: connect the hosted Renovate GitHub App to **this repository only** and inspect its onboarding pull request before accepting any configuration.
