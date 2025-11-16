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
via `.github/workflows/release-on-main.yml`.

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

Because we use **fast-forward merges**, maintainers must:

1. Ensure commits landing on `main`, `rc`, `beta`, or `alpha` use Conventional Commit messages.
2. Use `feat!:` or `BREAKING CHANGE:` when introducing breaking changes.
3. Merge locally with:

   ```bash
   git checkout main
   git fetch origin
   git merge --ff-only feature/my-feature
   git push origin main
   ```
4. Pushing to the appropriate branch will:

   * Run the tests.
   * Generate a new semver tag.
   * Create a GitHub Release.

Stable releases come from `main`.
Pre-releases come from `rc`, `beta`, and `alpha`.

---

## Communication

* Use [GitHub Issues](https://github.com/jcandan/ddev-ai-agent/issues) for bug reports and enhancements.
* PRs are welcome; include reasoning for architectural choices.
* Keep commits atomic and descriptive; use Conventional Commits (`feat:`, `fix:`, `chore:`, `ci:`, etc.).

---

## Maintainers

DO NOT MERGE PRs VIA THE GITHUB UI.

While `alpha`, `beta`, or `rc` is default branch, swap below `main` for that
pre-release branch.

GitHub does not support squash-and-fast-forward merges, and this repository
expects a **linear `main` history** where each change lands as a single,
clean, Conventional Commit.

When a PR is ready:

1. Confirm the **tests workflow is green**.
2. Make sure the resulting commit will use a proper **Conventional Commit**
   message so the automated release workflow can determine the correct bump.
3. Check out `main` and ensure it is up to date:

   ```bash
   git checkout main
   git fetch origin
   git reset --hard origin/main
   ```

4. Squash-merge the PR branch directly onto `main`:

   ```bash
   git merge --squash feature/my-branch
   ```

   This stages all changes without creating a merge commit.

5. Commit once with a Conventional Commit message **and a closing footer**:

   ```bash
   git commit -m "feat: add searxng integration

   Closes #123"
   ```

   Use `feat!:`, `fix!:`, or a `BREAKING CHANGE:` footer for breaking changes.

6. Push to `main`:

   ```bash
   git push origin main
   ```

The commit footer (`Closes #123`) will automatically close the associated
issue or PR. The PR will appear as **Closed** (not “Merged”), which is
expected for this workflow.

Pushing to `main` will:

* Re-run the tests
* Trigger `release-on-main.yml`:

  * Compute the next SemVer version from the commit message
  * Push the tag
  * Create a GitHub Release with generated release notes

Keep the history clean and readable. Each change should result in a single
Conventional Commit on `main`, `rc`, `beta`, or `alpha`.
