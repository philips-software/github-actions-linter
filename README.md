# Github Actions validator

This repository contains an action to validate the Github Actions workflows.

Under the hoods, it uses `action-validator` by [*mpalmer*](https://github.com/mpalmer/action-validator).

The action steps are:

- Check if workflow files have been modified, the below steps run only if this is true
- Download `action-validator` using ASDF
- Lint the workflows, and generate an output with the linted (and errored) files
- Post this output on a PR comment
- Cleanup the possibly modified files
