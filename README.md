
# git-client-hook
---

[![Build Status](https://travis-ci.org/adispring/git-client-hook.svg?branch=master)](https://travis-ci.org/adispring/git-client-hook)

git-client-hook used for installing some [git client hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) into node project.

## Function of git client hook

### `pre-push`: run npm test before git push

1. If `npm test` passed, git push will go on, else git push will be cancelled, and output errors npm test found.
2. Check if the pushing branch is update to newest remotes/origion/master.

### `commit-msg`: add branch name(jira) to commit message automatically & do some check

1. Check if commit-msg is valid, check options as follows:
    * if commit on branch: **master/develop/release/test**, it will output a warning.
    * if **commit without any message**, this commit will fail.
    * if branch name format does not satisfy `[feature | release | bugfix | hotfix]/JIRA-xxxx-brief-description`, this commit will fail. ([correct branch name format](https://confluence.atlassian.com/bitbucketserver/using-branches-in-bitbucket-server-776639968.html))
    * if commit message already contains a jira task number(regex: `[A-Z][A-Z_0-9]+-[0-9]+`), it will not add one again.
2. Add JIRA TASK number(branch name) to commit-msg.

If all above passed, **commit-msg will add branch name (JIRA TASK number) to commit message**.

[Add JIRA Task number into commit message](https://confluence.atlassian.com/display/FISHEYE/Using+Smart+Commits) can build a link between git commit and JIRA TASK.

## HOW TO USE git-client-hook

### Install

Run `npm install git-client-hook --save-dev`, pre-push & commit-msg hooks
will install into `git/hooks`.

Before install git hooks to `.git/hooks`, there are also some checks,

Check options as follows:

1. Git hook will install only if NODE_ENV is development or undefined.
2. If project is managed by git.(has .git/ dir) 
3. Check if some hooks have already installed, if hooks have changed/updated.
If hook has already installed and not changed, this hook will not install again,
else if it updated, it will updated.

### Uninstall

Run `npm uninstall git-client-hook --save-dev`:

1. git-client-hook will be uninstalled.
2. git hooks installed into .git/hooks/ will be uninstalled.

## Development

You can add any fantasy git client hooks to the repository, or custom it for your project.

Tests are executed using [Bats](https://github.com/sstephenson/bats)

Please feel free to submit pull requests and file bugs on the [issue
tracker](https://github.com/adispring/git-client-hook/issues).

## TODO

1. add post-commit to run npm test in background
2. add git-hook CLI: 1.custom pre-commit email verify; 2.batch change current branch commits email 

## DONE

### v0.0.24

1. optimize "feature branch update to master branch before push".

### v0.0.23

1. add branch name format reference link.
2. add branch name of `release`.

### v0.0.22

1. restrict branch name format: [feature | bugfix | hotfix]/JIRA-xxxx-brief-description.
2. optimize emoji.

### v0.0.21

1. fix: JIRA Number can not prepend to commit-msg when `git commit` with editor.

### v0.0.20
### v0.0.19

1. force feature branch update to master branch before git push.

### v0.0.18

1. check whether feature branch merged/rebased newest master branch in pre-push.

### v0.0.17
### v0.0.16
1. add travis ci
2. fix shell if [[ to [
3. fix some unit test warning

### v0.0.15
1. fix mulit jira task add to commit-msg when open editor to edit commit message
2. optimize jira number check: 
    - check jira number instead of branch name
    - add jira number instead of branch name to commit-msg.

### v0.0.14
1. add increment eslint check
2. fix typo: romte_sha -> remote_sha
3. change install approach
4. backup all exist and affected hooks

### v0.0.13
1. fix is_commit_msg_empty func in commit-msg

### v0.0.12
### v0.0.11
### v0.0.10
1. modify README.md

### v0.0.9
1. add git hook update unit-test
2. rename unit test dir name

### v0.0.8
1. change prepare-commit-msg to commit-msg. prepare-commit-msg invoked before editor, commit-msg invoked after user enters a commit message.
2. optimize string judgement, fix string var bug: $() to "$()"
3. fix empty commit-msg judegment.

### v0.0.6
1. uninstall git hooks from .git/hooks/ when uninstall git-client-hook
2. fix NODE_ENV does not work when bash git-hook-install.sh.

### v0.0.5
1. remove bats & bats-assert from git-hook-install.sh

### v0.0.4
1. update git unit test for new git-hook-install.sh

