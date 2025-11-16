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

Pushes to the `main` branch can automatically create a new Git tag and release
via `.github/workflows/release-on-main.yml`.

We use:

* [`mathieudutour/github-tag-action`](https://github.com/mathieudutour/github-tag-action)
  to compute and push the next SemVer tag based on commit messages.
* [`ncipollo/release-action`](https://github.com/ncipollo/release-action)
  to create a GitHub Release, using GitHub’s **“Generate release notes”**
  feature.

### Tag format

* Tags are plain SemVer: `0.1.0`, `0.2.0`, `1.0.0`, etc.
* There is **no** `v` prefix (to match other DDEV add-ons and the DDEV add-on
  registry).

### Commit message rules (Conventional Commits)

Version bumps are driven by **Conventional Commits** on `main`. In practice:

* `fix: ...` → **patch** bump (e.g. `0.1.3` → `0.1.4`)
* `feat: ...` → **minor** bump (e.g. `0.1.3` → `0.2.0`)
* `feat!: ...` or `fix!: ...` or a footer with `BREAKING CHANGE:`
  → **major** bump (e.g. `0.1.3` → `1.0.0`)

Other types such as `chore:`, `ci:`, `docs:`, `refactor:` generally do
**not** cause a version bump on their own.

Because we use fast-forward merges, maintainers are expected to make sure the
commits that land on `main` follow this pattern. That can mean:

* Enforcing Conventional Commits in branches/PRs, or
* Cleaning up commit messages (e.g. interactive rebase) before fast-forwarding
  to `main`.

When a new SemVer tag is created, `release-on-main` also creates a
GitHub Release and uses GitHub’s **generated release notes**, based on
the commits and merged PRs since the previous tag.

---

## Communication

* Use [GitHub Issues](https://github.com/jcandan/ddev-ai-agent/issues) for bug reports and enhancements.
* PRs are welcome; include reasoning for architectural choices.
* Keep commits atomic and descriptive; use Conventional Commits (`feat:`, `fix:`, `chore:`, `ci:`, etc.).

---

## Maintainers

DO NOT MERGE PRs VIA THE GITHUB GUI.

GitHub does not provide a fast-forward merge option for pull requests, and this
repository expects a **linear `main` history**.

When a PR is ready:

1. Confirm the **tests workflow is green** for that PR.
2. Ensure the commits that will land on `main` use Conventional Commit-style
   messages so the release bump is appropriate:

   * `feat:` for new features,
   * `fix:` for bug fixes,
   * `feat!:`, `fix!:`, or `BREAKING CHANGE:` for breaking changes,
   * `chore:`, `ci:`, `docs:`, etc. for non-release-impacting work.
3. Perform a **fast-forward merge** locally into `main`:

   ```bash
   git checkout main
   git fetch origin
   git merge --ff-only feature/my-branch
   git push origin main
   ```
4. The push to `main` will:

   * Re-run the tests on `main`.
   * Run `release-on-main`, which:

     * Computes the next SemVer tag from commit messages.
     * Pushes that tag.
     * Creates a GitHub Release with generated release notes.

Keep the history clean and readable. Small, focused commits with clear,
Conventional Commit-style messages make it much easier to understand and
trust the automated releases.
