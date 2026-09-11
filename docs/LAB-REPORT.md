# Renovate Dependency Automation Lab

## Objective

Evaluate Renovate in a controlled GitHub repository without affecting production, portfolio, application, or game repositories, and verify whether CI can distinguish safe dependency updates from breaking upgrades.

## Isolation strategy

- Dedicated repository: `rhynocerus/renovate-lab`
- Repository visibility: private during the experiment
- Hosted Mend Renovate GitHub App restricted to this repository only
- No Docker daemon or local Renovate service required
- No automatic merging enabled
- No production secrets or application code stored in the lab

This design intentionally keeps the experiment independent from the developer workstation and from unrelated active repositories.

## Test fixture

The repository contains a minimal Node.js project with deliberately outdated dependencies so Renovate has known update candidates to discover.

Initial dependencies:

- `chalk` 4.1.2
- `eslint` 8.57.0
- `prettier` 3.2.5
- `vite` 5.4.0

The project is marked `private: true` in `package.json` to prevent accidental npm publication.

## Renovate configuration

Renovate initially opened onboarding pull request [#1](../pull/1), proposing a `renovate.json` file based on the `config:recommended` preset.

After the CI preparation branch was merged, the onboarding branch became stale. During branch synchronization the onboarding PR was automatically closed. The approved configuration was then committed directly to `main`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "config:recommended"
  ]
}
```

Once the configuration existed on `main`, Renovate activated normally and created the Dependency Dashboard as issue [#5](../issues/5).

## Dependency discovery

Renovate detected both npm dependencies and GitHub Actions dependencies.

### npm

- `chalk` 4.1.2 → 6.0.0
- `eslint` 8.57.0 → 8.57.1 and 10.10.0
- `prettier` 3.2.5 → 3.9.6
- `vite` 5.4.0 → 5.4.21 and 8.3.0

### GitHub Actions

- `actions/checkout` v4 → v7
- `actions/setup-node` v4 → v7
- Node.js 22 → 24

The default hosted Renovate configuration rate-limited pull-request creation to two per hour, allowing the proposals to arrive gradually instead of flooding the repository.

## Automated validation

A GitHub Actions workflow named `Renovate Lab CI` validates dependency update pull requests independently of the developer workstation.

The workflow:

- checks out the repository;
- runs on Node.js 22;
- installs dependencies without lifecycle scripts;
- launches the small test application;
- checks formatting with Prettier;
- verifies that ESLint and Vite execute correctly.

During creation of the workflow, CI itself exposed two useful setup problems: npm caching could not be used without a lockfile, and the initial source file did not satisfy Prettier. Both were corrected before the dependency experiment began. The final baseline CI run passed successfully.

## Experiment result A: safe patch update

Renovate opened pull request [#3](../pull/3):

`eslint 8.57.0 → 8.57.1`

Characteristics:

- one-line dependency change;
- same major version;
- CI completed successfully;
- application smoke test remained functional;
- update was manually reviewed and merged.

**Result:** accepted.

This demonstrates the low-friction path for a compatible patch update: Renovate discovers and proposes the change, CI validates it, and a human reviewer approves the merge.

## Experiment result B: breaking major update

A rate-limited major update was deliberately requested from the Dependency Dashboard. Renovate opened pull request [#6](../pull/6):

`chalk 4.1.2 → 6.0.0`

Dependency installation completed successfully, but the runtime smoke test failed during `npm start` with:

```text
TypeError: chalk.cyan is not a function
```

The test application uses the CommonJS-era Chalk 4 API while newer Chalk versions changed module/API behavior. Renovate correctly proposed the available version, but the automated update alone could not guarantee application compatibility.

The failed CI signal prevented an unsafe merge. The PR was documented and closed without merging.

**Result:** rejected.

This is the most important finding of the laboratory: dependency automation must be paired with automated tests and human review, especially for major-version upgrades.

## Additional observed update

Renovate also opened pull request [#4](../pull/4) for:

`vite 5.4.0 → 5.4.21`

Its CI validation completed successfully. It remains useful as an additional same-major comparison case and does not need to be merged merely to prove that Renovate works.

## Findings

1. Hosted Renovate can be evaluated without installing Docker or running a local service.
2. Repository-scoped GitHub App permissions provide strong isolation for a pilot deployment.
3. The Dependency Dashboard gives a useful centralized inventory of pending dependency work.
4. Rate limiting prevents a newly enabled bot from flooding a repository with pull requests.
5. Patch/same-major updates can be low-risk when CI is green and the diff is small.
6. Major updates can install successfully while still breaking the application at runtime.
7. CI is therefore a required control, not an optional accessory, for dependency automation.
8. Automatic merging should remain disabled until a project's test coverage and update policy justify it.

## Recommended policy for real projects

For future repositories, a conservative starting policy is:

- enable Renovate only on explicitly selected repositories;
- retain `config:recommended` as a baseline;
- keep automerge disabled initially;
- review major updates manually;
- require CI before merging dependency PRs;
- adopt patch/minor automation gradually only after the test suite proves reliable;
- treat release notes and migration guides as part of the review for major upgrades.

## Portfolio value

This laboratory demonstrates practical familiarity with:

- dependency lifecycle management;
- GitHub pull-request workflows;
- CI validation;
- software supply-chain hygiene;
- semantic-version risk assessment;
- GitHub Actions;
- controlled automation policies;
- failure analysis and technical documentation.

A fork of the upstream `renovatebot/renovate` repository is not required merely to use Renovate. An upstream fork should be created only if this laboratory leads to a concrete code, test, bug fix, or documentation contribution to Renovate itself.

## Status

- [x] Private isolated repository created
- [x] Deliberately outdated dependency fixture created
- [x] Renovate GitHub App restricted to this repository
- [x] Onboarding configuration reviewed
- [x] GitHub Actions smoke-test CI implemented and validated
- [x] Renovate activated
- [x] Dependency Dashboard generated
- [x] Compatible patch update tested and merged
- [x] Breaking major update tested and rejected
- [x] Successful and failing update paths compared
- [x] Initial technical conclusions documented
- [ ] Repository privacy and presentation reviewed
- [ ] Repository made public for portfolio use
- [ ] Repository highlighted from the GitHub profile
