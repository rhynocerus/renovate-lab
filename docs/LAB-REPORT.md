# Renovate Dependency Automation Lab

## Objective

Evaluate Renovate in a controlled GitHub repository without affecting production, portfolio, application, or game repositories.

## Isolation strategy

- Dedicated repository: `rhynocerus/renovate-lab`
- Repository visibility: private during the experiment
- No Docker required on the workstation
- Renovate GitHub App access restricted to this laboratory repository
- No automatic merging enabled during the initial evaluation
- No production secrets or application code are stored here

## Test fixture

The repository contains a minimal Node.js project with deliberately outdated dependencies so Renovate has known update candidates to discover.

Current test dependencies:

- `chalk` 4.1.2
- `eslint` 8.57.0
- `prettier` 3.2.5
- `vite` 5.4.0

The project is marked `private: true` in `package.json` to prevent accidental npm publication.

## Renovate onboarding result

Renovate successfully detected `package.json` and opened onboarding pull request #1, `chore: Configure Renovate`.

The onboarding PR adds only `renovate.json` with the `config:recommended` preset. Renovate predicted six dependency update pull requests after activation, including both same-major updates and major-version upgrades.

## Automated validation

A GitHub Actions smoke-test workflow is used to evaluate update pull requests independently of the developer workstation. The workflow:

- checks out the repository;
- runs on Node.js 22;
- installs the dependency fixture without lifecycle scripts;
- starts the small test application;
- checks Prettier;
- verifies that ESLint and Vite execute correctly.

This separates dependency automation from the local Linux environment and provides a repeatable CI signal for every pull request.

## Experiment phases

1. Create isolated private repository.
2. Add dependency test fixture.
3. Install the hosted Renovate GitHub App with access only to this repository.
4. Inspect Renovate's onboarding pull request before merging.
5. Add CI smoke tests for dependency-update validation.
6. Activate Renovate by merging the onboarding PR.
7. Observe the Dependency Dashboard and generated dependency update PRs.
8. Review at least one same-major update and one major-version update without auto-merging majors.
9. Compare successful and breaking update behaviour.
10. Record findings, risks, and recommendations.
11. Publish the laboratory repository as a portfolio artifact once no private material is present.

## Safety policy

During the pilot:

- automatic merge remains disabled;
- major updates require manual review;
- Renovate access remains restricted to `renovate-lab`;
- no other repository is part of the experiment;
- no Docker daemon or local Renovate service is required.

## Portfolio value

The completed laboratory is intended to demonstrate practical familiarity with:

- dependency lifecycle management;
- GitHub pull-request workflows;
- CI validation;
- software supply-chain hygiene;
- controlled automation policies;
- review of semantic versioning changes;
- documentation of technical experiments.

A fork of the upstream `renovatebot/renovate` repository is not required merely to use Renovate. An upstream fork should be created only if the laboratory leads to a concrete code, test, or documentation contribution to Renovate itself.

## Status

- [x] Private repository created
- [x] Minimal dependency fixture created
- [x] Renovate GitHub App connected to this repository only
- [x] Onboarding pull request reviewed
- [x] CI smoke-test workflow prepared
- [ ] CI workflow reviewed and merged
- [ ] Renovate configuration approved and merged
- [ ] First dependency update proposals reviewed
- [ ] Same-major and major update behaviour compared
- [ ] Final conclusions documented
- [ ] Repository reviewed and made public for portfolio use
