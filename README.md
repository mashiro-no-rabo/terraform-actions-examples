# GitHub Actions for Terraform

Here are some GitHub Actions that can be useful when using Terraform.

Note that GitHub Actions might be updated so some assumptions/workarounds here might not work or be necessary in the future. Consult official documentation and experiment yourself to fix but these should be a good enough base implementation.

The general assumptions is that all Terraform code resides under `terraform` directory.

## Format and Commit Fixes

[Action for repo](.github/workflows/commit-format.yaml) and [Action for forks](.github/workflows/commit-format-fork.yaml).

The overall idea is to auto format any incoming PRs - but not directly commit to `master` branch, also such format fixing commits should be squashed into other _real_ changes.

However the first caveat is that the `on: pull_request` trigger does not work for PRs coming from forks. To workaround this another workflow is added with `on: push` trigger so it runs on all checkins but limit with `if:` to exclude the original repo, the fork might still need to enable GitHub Actions for this to work, but this ensures all commits in PRs will be auto formatted.

Another caveat is that any format commit will not trigger actions (in current GitHub Actions design), so either use TeamCity for status check or requires the commit to be rebased (like above code).

One potential extension is to use https://github.com/actions/github-script and create "suggestion blocks" so that users can accept formatting changes themselves, though it might be rather complex to implement and for example if the `master` branch is not formatted correctly then the suggestions won't have a place to land.

## Format and Report Failure

[Action](.github/workflows/report-format.yaml)

Basically reuse the above action but the trigger can be simplified so only one action is needed, and it errors out if some files are not formatted. You might want to also use the [GitHub Super-Linter](https://github.com/github/super-linter).
