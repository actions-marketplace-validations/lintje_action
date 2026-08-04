# Lintje GitHub Action

Validate Git commits on every push with GitHub Actions. Learn more about Lintje
on the [Lintje.dev website][website].

<div align="center">
  <b><a href="https://lintje.dev">Lintje.dev website</a> | <a href="https://github.com/marketplace/actions/lintje">Lintje on GitHub marketplace</a></b>
</div>

---

## Table of Contents

- [Overview](#overview)
- [Usage](#usage)
    - [Checkout fetch-depth](#checkout-fetch-depth)
    - [Tag version number](#tag-version-number)
- [Configuration](#configuration)
    - [Example configuration](#example-configuration)
- [Development](#development)
- [Code of Conduct](#code-of-conduct)

## Overview

Lintje is a Git linter for people to help write individuals and team write
better commits. Visit the [Lintje.dev website][website] to learn more about how
to use Lintje and the [rules it uses to validate commits and
branches](https://lintje.dev/docs/rules/).

Using this GitHub Action Lintje will automatically validate the pushed Git
commit on the repository, and multiple commits if more than one was pushed.

For Pull Requests it will not validate all commits in the Pull Request.
Previously pushed commits may have already failed previous builds, which will
also fail the build when the branch is merged.

## Usage

Create a new [GitHub Actions
workflow](https://docs.github.com/en/actions/quickstart) or add it to an
existing workflow that already does testing and linting steps.

Add steps that uses the `actions/checkout@v7` and
`lintje/action@v0.11` actions like shown below.

```yaml
name: "Your workflow name"
on: [push]

jobs:
  lintje: # Add a new job for Lintje
    name: "Lintje (Git linter)"
    runs-on: ubuntu-latest # Supported on ubuntu, macOS and Microsoft Windows
    steps:
      - uses: actions/checkout@v7
        with:
          fetch-depth: 0 # Fetch depth is required
      - uses: lintje/action@v0.11
```

### Checkout fetch-depth

Configure the `actions/checkout@v7` action to use `fetch-depth: 0` to fetch the
entire Git history of the repository. By default the checkout action only
fetches the last commit, which makes it impossible for Lintje to test multiple
commits if more than one commit was pushed. The `fetch-depth: 0` value means
the entire Git history gets fetched.

You can also choose to set it to another value that's high enough to fetch all
you'll ever push, like `fetch-depth: 100`, if you never push more than 100
commits at a time.

### Tag version number

The tag for the Lintje action `v#.#.#` corresponds to the Lintje release with
the same version number. Upgrade Lintje in your build by updating the version
number.

## Configuration

Like Lintje itself, the Lintje GitHub Action has minimal configuration.
The following configuration options are available.

- `branch_validation` (Default value: `true`):
    - Configure Lintje's Git branch validation.
      Setting this to `false` is the equivalent of calling `lintje --no-branch`.
      Read more about the [`--no-branch` CLI
      flag](https://lintje.dev/docs/usage/#branch-validation).
- `hints` (Default value: `true`):
    - Configure Lintje's hints output. Hints will not fail the validation.
      Settings this to `false` is the equivalent of calling `lintje --no-hints`.
      Read more about the [`--no-hints` CLI
      flag](https://lintje.dev/docs/usage/#hints).
- `color` (Default value: `true`):
    - Configure Lintje's colorized output.
      Setting this to `false` is the equivalent of calling `lintje --no-color`.
      Read more about the [`--no-color` CLI
      flag](https://lintje.dev/docs/usage/#colorized-output).
- `verbose` (Default value: `false`):
    - Configure Lintje's verbose mode.
      Setting this to `true` is the equivalent of calling `lintje --verbose`.
      Read more about the [`--verbose` CLI
      flag](https://lintje.dev/docs/usage/#verbose-output).

Read more about [how to configure
Lintje](https://lintje.dev/docs/configuration/).

### Example configuration

```yaml
- uses: lintje/action@v0.11
  inputs:
    branch_validation: false # Turn off branch validation. On by default
    hints: false # Turn off hints. On by default
```

## Development

To update the Lintje GitHub Action to a new Lintje release, run the
["Update Lintje action"
workflow](https://github.com/lintje/action/actions/workflows/update_action.yml)
from the `main` branch with the version number of the Lintje release. The
workflow will update the version number, the checksums, the `README.md` and
`CHANGELOG.md` files, rebuild the Action and create the release and tags.

If the update requires changes to the Action's behavior or configuration,
make those changes in a separate commit first.

To publish a new release with only changes to the Action itself, e.g. when no
new Lintje release is available, run the ["Release Lintje
action" workflow](https://github.com/lintje/action/actions/workflows/release_action.yml)
from the `main` branch with a version number in the
`v<lintje-version>-<suffix>` format, e.g. `v0.11.3-4`. Add a `CHANGELOG.md`
entry for the release version first. The workflow will rebuild the Action,
create the release and tag.

## Code of Conduct

This project has a [Code of Conduct](CODE_OF_CONDUCT.md) and contributors are
expected to adhere to it.

[website]: https://lintje.dev
[installation]: https://lintje.dev/docs/installation/
