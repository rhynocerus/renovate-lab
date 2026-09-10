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

## Experiment phases

1. Create isolated private repository.
2. Add dependency test fixture.
3. Install the hosted Renovate GitHub App with access only to this repository.
4. Inspect Renovate's onboarding pull request before merging.
5. Activate Renovate by merging the onboarding PR.
6. Observe the Dependency Dashboard and generated dependency update PRs.
7. Review at least one same-major update and one major-version update without auto-merging majors.
8. Record findings, risks, and recommendations.
9. Decide whether the lab is suitable to publish as a portfolio project.

## Safety policy

During the pilot:

- automatic merge remains disabled;
- major updates require manual review;
- Renovate access remains restricted to `renovate-lab`;
- no other repository is part of the experiment.

## Status

- [x] Private repository created
- [x] Minimal dependency fixture created
- [x] Renovate GitHub App connected to this repository only
- [x] Onboarding pull request reviewed
- [ ] Renovate configuration approved and merged
- [ ] First dependency update proposals reviewed
- [ ] Final conclusions documented
