# Renovate Dependency Automation Lab

## Objective

Evaluate Renovate in a controlled GitHub repository without affecting production, portfolio, application, or game repositories.

## Isolation strategy

- Dedicated repository: `rhynocerus/renovate-lab`
- Repository visibility: private during the experiment
- No Docker required on the workstation
- No access intended for PerData, Packet Runner, or other repositories
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

## Experiment phases

1. Create isolated private repository.
2. Add dependency test fixture.
3. Install the hosted Renovate GitHub App with access only to this repository.
4. Inspect Renovate's onboarding pull request before merging.
5. Configure conservative update policies.
6. Observe dependency discovery and proposed updates.
7. Review generated pull requests without auto-merging.
8. Record findings, risks, and recommendations.
9. Decide whether the lab is suitable to publish as a portfolio project.

## Safety policy

During the pilot:

- automatic merge remains disabled;
- major updates will require manual review;
- Renovate access must be restricted to `renovate-lab` only;
- no other user repository is part of the experiment.

## Status

- [x] Private repository created
- [x] Minimal dependency fixture created
- [ ] Renovate GitHub App connected to this repository only
- [ ] Onboarding pull request reviewed
- [ ] Renovate configuration approved
- [ ] First dependency update proposals reviewed
- [ ] Final conclusions documented
