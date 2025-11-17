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

### 4. Install the add-on from your local path

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

This repository uses `.github/workflows/tests.yml` to run the DDEV add-on
test matrix via `ddev/github-action-add-on-test`.

The tests run:

* On every **pull request** (except changes that only touch `*.md` files).
* On every **push to `main`**.
* On a nightly **schedule**.
* On manual **workflow_dispatch** (with optional `debug_enabled`).

The matrix currently runs against:

* `ddev_version: stable`
* `ddev_version: HEAD`

Before merging a PR, ensure the **tests workflow is green**. Since `main`
can trigger releases, `main` must always be in a releasable state.

See [`tests/test.bats`](tests/test.bats) for the test entrypoint.

---

## Releases

Pushes to certain branches automatically create SemVer tags and releases
via `.github/workflows/release-on-push-release-branch.yml`.

We use:

* [`mathieudutour/github-tag-action`](https://github.com/mathieudutour/github-tag-action)
  to compute and push the next SemVer tag based on commit messages.
* [`ncipollo/release-action`](https://github.com/ncipollo/release-action)
  to create a GitHub Release, using GitHub’s **“Generate release notes”**
  feature.

### Commit message rules (Conventional Commits)

Version bumps are driven by **Conventional Commits** on `main`. In practice:

* `fix: ...` → **patch** bump (e.g. `0.1.3` → `0.1.4`)
* `feat: ...` → **minor** bump (e.g. `0.1.3` → `0.2.0`)
* `feat!: ...` or `fix!: ...` or a footer with `BREAKING CHANGE:`
  → **major** bump (e.g. `0.1.3` → `1.0.0`)

Other types such as `chore:`, `ci:`, `docs:`, `refactor:` generally do
**not** cause a version bump on their own.

### Release Channels

The branch you push to determines the release channel:

| Branch  | Release Type      | Tag Examples     |
| ------- | ----------------- | ---------------- |
| `main`  | stable            | `0.3.0`, `1.0.1` |
| `rc`    | release candidate | `1.2.0-rc.1`     |
| `beta`  | beta prerelease   | `1.2.0-beta.3`   |
| `alpha` | alpha prerelease  | `1.2.0-alpha.1`  |

GitHub Releases use GitHub’s **generated release notes**.

### Workflow Expectations

Because we require a **linear history** and use **Conventional Commits** to drive
automated releases, maintainers should:

1. Ensure commits landing on `main`, `rc`, `beta`, or `alpha` follow Conventional
   Commit messages:
   - `feat:` for new features.
   - `fix:` for bug fixes.
   - `feat!:`, `fix!:`, or a `BREAKING CHANGE:` footer for breaking changes.
   - `chore:`, `ci:`, `docs:`, etc. for non-release-impacting work.
2. Merge PRs via the GitHub UI using **Squash and merge** (recommended) or
   **Rebase and merge**. Do **not** use “Create a merge commit”.
   - For **Squash and merge**, ensure the **PR title** is a Conventional Commit
     and include `Closes #NNN` in the description if you want to close an
     issue/PR.
   - For **Rebase and merge**, ensure all commits in the PR already follow
     Conventional Commits.
3. Pushing to `main`, `rc`, `beta`, or `alpha` will:
   - Run the tests.
   - Generate a new SemVer tag based on the commit messages.
   - Create a GitHub Release with generated notes.

Stable releases come from `main`.
Pre-releases come from `rc`, `beta`, and `alpha`.

---

## Communication

* Use [GitHub Issues](https://github.com/jcandan/ddev-ai-agent/issues) for bug reports and enhancements.
* PRs are welcome; include reasoning for architectural choices.
* Keep commits atomic and descriptive; use Conventional Commits (`feat:`, `fix:`, `chore:`, `ci:`, etc.).

---

## Maintainers

This repository expects a **linear `main` history** and uses **Conventional
Commits** to drive automated versioning.

To keep history clean and let the release workflow do the right thing:

1. **Do not use “Create a merge commit”** in the PR UI.
2. Prefer **“Squash and merge”**:
   - Set the PR title to a Conventional Commit (e.g. `feat: add searxng
     integration`).
   - Optionally add a footer like `Closes #123` in the PR description. GitHub
     will include this in the squashed commit body and automatically close the
     issue/PR.
   - Edit the commit message in the Squash dialog if needed before merging.
3. If you use **“Rebase and merge”**, ensure all commits already use
   Conventional Commit messages.
4. After the merge:
   - The push to `main` (or `rc` / `beta` / `alpha`) will rerun tests.
   - `release-on-push-release-branch.yml` will:
     - Compute the next SemVer version from the commit messages.
     - Push the tag.
     - Create a GitHub Release with generated release notes.

Keep the history small and readable. Each merged PR should result in a clear,
Conventional Commit on the release branch.
