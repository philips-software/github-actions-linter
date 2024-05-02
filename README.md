# Github Actions validator v2

This repository contains an action to validate the Github Actions workflows.

## Description

Under the hood, it uses https://github.com/rhysd/actionlint

The action steps are:

- Download `actionlint` using ASDF
- Lint the workflows, and generate an output with the linted (and errored) files
- Post this output on a PR comment

## Example

```yaml
permissions:
  contents: read # checkout
  pull-requests: write # post comment

jobs:
  actions-validator:
    name: Run Github Actions validator
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Validate Github Actions workflows
        uses: philips-software/github-actions-linter@v2
```

## ShellCheck rules

This action uses actionlint, which in turn uses [ShellCheck](https://www.shellcheck.net/wiki/) to validate script steps.

### Disable ShellCheck rules for all workflows

Although we don't recommend it, you can disable specific ShellCheck rules for all workflows checked, with the `shellcheck-disable-codes` input:

```yaml
- name: Validate Github Actions workflows
  uses: philips-software/github-actions-linter@v2
  with:
    shellcheck-disable-codes: SC2052,SC2034
```

### Disable a ShellCheck rule for a specific line

You can disable a specific rule directly within your script, using the [ShellCheck syntax](https://www.shellcheck.net/wiki/Ignore):

```yaml
# The workflow that gets validated
- name: Do stuff
  run: |
    foo=12
    # shellcheck disable=SC2090
    echo $foo
```

## Development

Github recommends having multiple kind of tags for the actions, a full SemVer tag (e.g. `vX.Y.Z`), as well as a major-only tag (e.g. `vX`).

When modifying this action if the changes:

- don't modify the functionalities, bump the patch version (e.g. `Z` in `vX.Y.Z`)
- add new functionalities, without breaking backward compatibility, bump the minor version and reset the patch version (e.g. `Y` in `X.Y.0`)
- break backward compatibility, bump the major version and reset the other versions (e.g. `X` in `vX.0.0`)

Note that after a version bump, the major-only tag must be updated as well.

> The major-only tag has to be force-pushed as Github doesn't allow to modify tags by default.

Full example:

```sh
git tag
# v1, v1.0.0

open action.yml
git commit -am "Chore: some patch modifications"

# we create the patch version tag
git tag v1.0.1
# and the major-only tag
git tag -f v1

git push origin v1.0.1
git push -f origin v1
```
