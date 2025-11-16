# Contributing to DDEV AI Agent

## Development workflow

### 1. Clone the repository

```bash
git clone git@github.com:jcandan/ddev-ai-agent.git
cd ddev-ai-agent
```

### 2. Fire it up in VSCode

```bash
code ddev-ai-agent
```

### 3. Create a QA DDEV project

Use a clean directory (outside this repo):

```bash
mkdir ~/projects/my-ai-agent
cd ~/projects/my-ai-agent
ddev config --project-name=my-ai-agent --project-type=generic --docroot=.
```

### 3. Install the add-on from your local path

```bash
ddev add-on get ./path/to/ddev-ai-agent
ddev start
```

You should be able to open:

https://my-ai-agent.ddev.site:5678/

---

## Iterating on changes

While editing the add-on repo, you can update your QA project with:

```bash
ddev add-on get ../ddev-ai-agent && ddev restart
```

This reapplies the current add-on definition and restarts the environment.

---

## Resources

* [DDEV Add-ons: Creating, maintaining, testing](https://www.youtube.com/watch?v=TmXqQe48iqE) (part of the [DDEV Contributor Live Training](https://ddev.com/blog/contributor-training))
* [Advanced Add-On Techniques](https://ddev.com/blog/advanced-add-on-contributor-training/)
* [DDEV Add-on Maintenance Guide](https://ddev.com/blog/ddev-add-on-maintenance-guide/)
* [DDEV Documentation for Add-ons](https://ddev.readthedocs.io/en/stable/users/extend/additional-services/)
* [DDEV Add-on Registry](https://addons.ddev.com/)

---

## Testing

This repository uses a GitHub Actions workflow (`.github/workflows/tests.yml`)
to run the add-on tests via `ddev/github-action-add-on-test`.

- Tests run on:
  - Every pull request.
  - Every push to `main`.
  - A nightly scheduled run.
  - Manual `workflow_dispatch` (with optional debug).

Before merging a PR, ensure the **tests workflow is green**. Since `main` auto-
releases on every push (see below), `main` must always be in a releasable state.

See [tests/test.bats] for the test entrypoint.

---

## Releases

Pushes to the `main` branch automatically create a new GitHub Release and
semantic version tag via `.github/workflows/release-on-main.yml` (using the
`Tag/Release on Push` action).

- By default, the workflow bumps the **minor** version (e.g. `v0.1.0` → `v0.2.0`).
- You can override the bump per PR using labels:
  - `release:major`
  - `release:minor`
  - `release:patch`
- You can skip a release by:
  - Adding the `norelease` label to the PR, or
  - Including `[norelease]` in the commit title.

Because every push to `main` produces a release, **all changes must land on
`main` via PRs with passing tests**.

---

## Communication

* Use [GitHub Issues](https://github.com/jcandan/ddev-ai-agent/issues) for bug reports and enhancements.
* PRs are welcome; include reasoning for architectural choices.
* Keep commits atomic and descriptive; use conventional commits.

## Maintainers

DO NOT MERGE PRs VIA GUI!

Surprisingly, GitHub does not provide a fast-forward merge options for pull
requests. When a PR is ready, perform a fast-forward merge locally.

<!-- @todo Add GitHub action to simplify fast-foward merges from PRs -->
