# Github Actions validator

This repository contains an action to validate the Github Actions workflows.

## Description

Under the hoods, it uses `action-validator` by [*mpalmer*](https://github.com/mpalmer/action-validator).

The action steps are:

- Check if workflow files have been modified, the below steps run only if this is true
- Download `action-validator` using ASDF
- Lint the workflows, and generate an output with the linted (and errored) files
- Post this output on a PR comment
- Cleanup the possibly modified files


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
