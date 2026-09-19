# bis-ci

Two reusable GitHub Actions workflows, shared by the BiS World of Warcraft addons
([BiSTheme](https://github.com/arnold891-eng/BiSTheme), [BiSTools](https://github.com/arnold891-eng/BiSTools),
[BiSGamba](https://github.com/arnold891-eng/BiSGamba), [BiSHealing](https://github.com/arnold891-eng/BiSHealing),
[BiSInnervate](https://github.com/arnold891-eng/BiSInnervate), [Nebbinator](https://github.com/arnold891-eng/Nebbinator),
[BiSMemories](https://github.com/arnold891-eng/BiSMemories) and three private ones).

| workflow | what it does |
|---|---|
| `check.yml` | every dev suite, bislint and the API fence for one addon |
| `release.yml` | a tag `vX.Y.Z` builds the zip and uploads it to CurseForge; anything else is a dry run |

Each addon calls one with a nine-line caller:

```yaml
jobs:
  check:
    uses: arnold891-eng/bis-ci/.github/workflows/check.yml@main
    with:
      addon: BiSTools
    secrets:
      BISDEV_TOKEN: ${{ secrets.BISDEV_TOKEN }}
```

## Why this repo is public, and holds nothing else

A **public repository cannot `uses:` a reusable workflow that lives in a private one.** There is
no setting for it — the private repo's Actions access was already `user`, the permissive value,
and every run still died before a single job started, with the only message GitHub gives you:

> This run likely failed because of a workflow file issue.

No job, so no log: `gh run view --log-failed` answers *log not found*. Nothing in it says
"visibility". Seven repos went red the minute they were made public.

So the workflows live here, in a repo that is public and contains **only** these workflows. The
toolbox they run (`check.sh`, bislint, the API fence, the canonical libraries) stays private and
is cloned by the jobs with a token.

One copy, ten callers: a syntax error here breaks every BiS repo at once. `main` is protected,
and `self.yml` checks on every pull request that both files parse, are still callable, and still
name the job the callers' branch protection requires.
