# Renovate Dependency Automation Lab

[![Renovate Lab CI](https://github.com/rhynocerus/renovate-lab/actions/workflows/ci.yml/badge.svg)](https://github.com/rhynocerus/renovate-lab/actions/workflows/ci.yml)
![Renovate](https://img.shields.io/badge/Renovate-enabled-1A1F6C?logo=renovatebot&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-22-339933?logo=nodedotjs&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/CI-GitHub%20Actions-2088FF?logo=githubactions&logoColor=white)

Controlled GitHub laboratory for evaluating automated dependency management with Renovate, CI validation, and human review.

## What this lab demonstrates

This repository was created as an isolated test environment rather than enabling Renovate directly on an active application.

The experiment uses the hosted Renovate GitHub App, deliberately outdated Node.js dependencies, and GitHub Actions smoke tests to observe both successful and breaking dependency upgrades without requiring Docker or a local Renovate service.

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

## Results

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

CI therefore exposed an API/module compatibility problem before the dependency could reach `main`. The PR was documented and closed without merging.

This contrast is the central result of the laboratory: **Renovate automates discovery and proposal, while CI and human review control whether a change is safe to accept.**

## Safety model

- Renovate access is restricted to this repository.
- Automatic dependency merging is disabled.
- The test package is marked `"private": true`.
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

## Full report

See [`docs/LAB-REPORT.md`](docs/LAB-REPORT.md) for the complete methodology, CI setup, observed Renovate behavior, failure analysis, conclusions, and recommendations for real projects.

## Portfolio purpose

The project documents hands-on work with dependency lifecycle management, GitHub Actions, pull-request review, software supply-chain hygiene, semantic-version risk, and controlled DevOps automation.

An upstream fork of `renovatebot/renovate` is intentionally **not** part of this lab. A fork would only be useful if a concrete bug fix, test, code change, or documentation contribution is identified for the Renovate project itself.

## Status

🧪 Core experiment completed. The repository remains private until its final portfolio review is complete.
