# Renovate Dependency Automation Lab

[![Renovate Lab CI](https://github.com/rhynocerus/renovate-lab/actions/workflows/ci.yml/badge.svg)](https://github.com/rhynocerus/renovate-lab/actions/workflows/ci.yml)
![Renovate](https://img.shields.io/badge/Renovate-enabled-1A1F6C?logo=renovatebot&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-22-339933?logo=nodedotjs&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/CI-GitHub%20Actions-2088FF?logo=githubactions&logoColor=white)

Public portfolio laboratory for evaluating automated dependency management with Renovate, GitHub Actions CI, and human review.

The goal is not merely to install a dependency bot. This repository demonstrates a controlled workflow in which Renovate discovers outdated dependencies and proposes changes, CI tests those changes, and a human reviewer decides whether they are safe to merge.

## What this lab demonstrates

This repository was deliberately isolated from active applications so dependency automation could be tested without risking production or portfolio code.

The experiment uses the hosted Renovate GitHub App, intentionally outdated Node.js dependencies, and GitHub Actions smoke tests. No Docker daemon or local Renovate service is required on the development workstation.

## Test workflow

```text
Outdated dependency
        ↓
     Renovate
        ↓
  Pull Request
        ↓
 GitHub Actions CI
        ↓
  ┌───────────────┐
  │               │
 green           red
  │               │
review          investigate
  │               │
merge           reject/fix
```

## Experiment results

| Case | Update | CI | Decision |
| --- | --- | --- | --- |
| Compatible patch | ESLint `8.57.0` → `8.57.1` | ✅ Passed | Merged |
| Same-major comparison | Vite `5.4.0` → `5.4.21` | ✅ Passed | Observed |
| Breaking major | Chalk `4.1.2` → `6.0.0` | ❌ Failed | Rejected |

### ✅ Compatible update

Pull request [#3](../../pull/3) updated ESLint from `8.57.0` to `8.57.1`.

- Renovate generated the PR automatically.
- CI passed.
- The diff was manually reviewed.
- The update was merged successfully.

### ❌ Breaking major update detected

Pull request [#6](../../pull/6) proposed Chalk `4.1.2` → `6.0.0`.

Installation succeeded, but the application smoke test failed at runtime with:

```text
TypeError: chalk.cyan is not a function
```

CI exposed an API/module compatibility problem before the dependency could reach `main`. The PR was documented and closed without merging.

This contrast is the central result of the laboratory: **Renovate automates discovery and proposal, while CI and human review control whether a change is safe to accept.**

## Safety model

- Renovate access is restricted to this repository.
- Automatic dependency merging is disabled.
- The test package is marked `"private": true` to prevent accidental npm publication.
- No production secrets or application source code are stored here.
- Docker is not required on the development workstation.
- Major dependency upgrades remain subject to manual review.

## Detected dependency sources

Renovate successfully discovered dependencies in both:

- `package.json` via the npm manager;
- `.github/workflows/ci.yml` via the GitHub Actions manager.

It also created a [Dependency Dashboard](../../issues/5) to track open, rate-limited, and detected updates.

## Repository structure

```text
renovate-lab/
├── .github/
│   └── workflows/
│       └── ci.yml
├── docs/
│   └── LAB-REPORT.md
├── src/
│   └── index.js
├── package.json
├── renovate.json
└── README.md
```

## Skills demonstrated

This project provides hands-on evidence of work with:

- dependency lifecycle management;
- GitHub pull-request workflows;
- GitHub Actions CI;
- semantic-version risk assessment;
- software supply-chain hygiene;
- controlled automation policies;
- failure analysis;
- technical documentation.

## Full report

See [`docs/LAB-REPORT.md`](docs/LAB-REPORT.md) for the complete methodology, CI setup, observed Renovate behavior, failure analysis, conclusions, and recommendations for real projects.

## Upstream contribution policy

An upstream fork of `renovatebot/renovate` is intentionally **not** part of this lab. A fork becomes useful only if the experiment identifies a concrete bug fix, test improvement, code change, or documentation contribution worth proposing upstream.

## Status

✅ Core experiment completed and published as a portfolio laboratory. Renovate remains active with automatic dependency merging disabled, so future update proposals can continue to be reviewed safely.
